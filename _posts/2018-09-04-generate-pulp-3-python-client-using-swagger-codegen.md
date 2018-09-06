---
title: Generate Pulp 3 Python client using swagger-codegen
author: Dennis Kliban
---
As a third party wishing to integrate with Pulp 3, you can use OpenAPI tooling to generate bindings
for over 20 different languages. Here is an example of how to use `swagger-codegen` to generate
Python bindings.

The REST API documentation for Pulp 3 is available on every Pulp instance at `/pulp/api/v3/docs`.
The documentation is auto generated using the OpenAPI 2.0, formerly known as Swagger, schema
definition. Each Pulp instance has a variety of plugins installed. The documentation is always a
reflection of the installed plugins.

The schema can also be used to generate client bindings in many languages using `swagger-codegen`.
The schema is available for your local Pulp at `/pulp/api/v3/docs/api.json`.

The ["Getting Started"](https://github.com/swagger-api/swagger-codegen#getting-started) section of
`swagger-codegen` documentation contains a link to the latest version of the jar that can be used
to generate a client for Pulp. The example below can be used to generate the Python client.

{% highlight bash %}
# Download swagger-codegen-cli jar
curl -o swagger-codegen-cli.jar http://central.maven.org/maven2/io/swagger/swagger-codegen-cli/2.3.1/swagger-codegen-cli-2.3.1.jar

# List available config options for Python clients
java -jar swagger-codegen-cli.jar config-help -l python


CONFIG OPTIONS
        packageName
            python package name (convention: snake_case). (Default: swagger_client)

        projectName
            python project name in setup.py (e.g. petstore-api).

        packageVersion
            python package version. (Default: 1.0.0)

        packageUrl
            python package URL.

        sortParamsByRequiredFlag
            Sort method arguments to place required parameters before optional parameters. (Default: true)

        hideGenerationTimestamp
            hides the timestamp when files were generated (Default: true)

        library
            library template (sub-template) to use (Default: urllib3)

# Create a custom swagger-codegen config for Python
vim config.json

{
  "packageName":"my_pulp3_client",
  "projectName":"my-pulp3-client",
  "packageVersion":"0.0.1"
}

# Download the OpenAPI 2.0 schema from Pulp
curl -o pulp-api.json http://localhost:8000/pulp/api/v3/docs/api.json

# Generate the client
java -jar swagger-codegen-cli.jar generate \
  -i pulp-api.json \
  -l python \
  -o ./my_pulp3_client \
  -c config.json

# Install the new Python package
pip install my_pulp3_client/
{% endhighlight %}

Below is an example of a script that uses the new client package to interact with Pulp.

{% highlight python %}
import my_pulp3_client
from my_pulp3_client import Distribution, FileContent, FilePublisher, FileRemote, Repository, \
    RepositorySyncURL, RepositoryPublishURL
from my_pulp3_client.rest import ApiException
from pprint import pprint
from time import sleep


def monitor_task(task_href):
    """Polls the Task API until the task is in a completed state.

    Prints the task details and a success or failure message. Exits on failure.

    Args:
        task_href(str): The href of the task to monitor

    Returns:
        list[str]: List of hrefs that identify resource created by the task

    """
    completed = ['completed', 'failed', 'canceled']
    task = api.tasks_read(task_href)
    while task.state not in completed:
        sleep(2)
        task = api.tasks_read(task_href)
    pprint(task)
    if task.state == 'completed':
        # TODO: https://pulp.plan.io/issues/3966
        # This client is not able to interpret the 'created resources' field correctly.
        print("The task was successfful.")
        return task.created_resources
    else:
        print("The task did not finish successfully.")
        exit()


# Configure HTTP basic authorization: basic
configuration = my_pulp3_client.Configuration()
configuration.username = 'admin'
configuration.password = 'admin'
configuration.safe_chars_for_path_param = '/'

client = my_pulp3_client.ApiClient(configuration)

# create an instance of the API class
api = my_pulp3_client.PulpApi(client)

try:
    # Create a File Remote
    remote_url = 'https://repos.fedorapeople.org/pulp/pulp/demo_repos/test_file_repo/PULP_MANIFEST'
    remote_data = FileRemote(name='bar', url=remote_url)
    file_remote = api.remotes_file_create(remote_data)
    pprint(file_remote)

    # Create a Repository
    repository_data = Repository(name='foo')
    repository = api.repositories_create(repository_data)
    pprint(repository)

    # Sync a Repository
    repository_sync_data = my_pulp3_client.RepositorySyncURL(repository=repository.href)
    sync_response = api.remotes_file_sync(file_remote.href, repository_sync_data)
    pprint(sync_response)

    # Monitor the sync task
    created_resources = monitor_task(sync_response.href)

    repository_version_1_href = created_resources[0]

    # Create an artifact from a local file
    artifact = api.artifacts_create('devel/pulp3_python_client_example/test_bindings.py')
    pprint(artifact)

    # Create a FileContent from the artifact
    file_data = FileContent(relative_path='foo.tar.gz', artifact=artifact.href)
    filecontent = api.content_file_files_create(file_data)
    pprint(filecontent)

    # Add the new FileContent to a repository version
    repo_version_data = {'add_content_units': [filecontent.href]}
    repo_version_response = api.repositories_versions_create(repository.href, repo_version_data)

    # Monitor the repo version creation task
    created_resources = monitor_task(repo_version_response.href)
    repository_version_2_href = created_resources[0]

    # Create a FilePublisher
    file_publisher_data = FilePublisher(name='bar')
    file_publisher = api.publishers_file_create(file_publisher_data)

    # Create a publication from the latest version of the repository
    publish_data = RepositoryPublishURL(repository=repository.href)
    publish_response = api.publishers_file_publish(file_publisher.href, publish_data)

    # Monitor the publish task
    created_resources = monitor_task(publish_response.href)
    publication_href = created_resources[0]

    distribution_data = Distribution(name='baz', base_path='foo', publication=publication_href)
    distribution = api.distributions_create(distribution_data)
except ApiException as e:
    print("Exception when calling the Api: %s\n" % e)
{% endhighlight %}
