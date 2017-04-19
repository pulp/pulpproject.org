---
title: Maintaining Community with Clear Goals
author: Brian Bouterse
---
At PulpCon 2016, the Pulp core developers and quality engineers wanted a
better way to describe what Pulp does. Through discussions we arrived at:

    "Acquire, Organize, and Distribute Software"

The core team felt that tagline got to the heart of Pulp's value proposition
for our users. It is both a statement of what we already feel Pulp does
and a yardstick to guide future decisions. Through this tagline, we can better
maintain and serve the Pulp community by focusing on features and user
experiences within this area. We also use it to recognize opportunities to
collaborate with other software communities that are focused on other areas.
That tagline motivates a never ending goal: to be the software that Acquires,
Organizes, and Distributes Software the best.

One example of how this goal is shaping the Pulp community is seen with Pulp
3.0, which disincludes consumer management features. These features were
provided in Pulp 2.y such as associating repositories with machines or
triggering package installations. Pulp was providing those features but not as
robustly as some of the configuration management options out there like
[Ansible](https://www.ansible.com/) or [mgmt](https://github.com/purpleidea/mgmt/).
This decision was debated heavily because Pulp users like the consumer
management features, but it is challenging to make first-class consumer
management features without a dedicated focus in that area. After articulating
a clear goal and recognizing that consumer management isn't it, the decision
to disinclude those features in the next major release made more sense.

Another example can be seen with the Pulp tasking system which is powered by
[Celery](http://www.celeryproject.org/). Celery is a community dedicated to
providing a great, asynchronous Python tasking paradigm. It's critically
important to Pulp to have an excellent, asynchronous tasking system, but it is
not important if Pulp is creating that tasking system or using something else.
Looking back at the decision to stop using a home-grown tasking system and
start using Celery with Pulp 2.4  was the right decision. It has benefitted
the community greatly because Pulp contributors can focus on supporting the
Pulp value proposition more directly. It also doesn't hurt that Celery is a
great tasking system.

Consider what goals exist for communities you participate in. Are they clearly
articulated? Are they written down? Having clear goals will support better
decision making and more focus on creating the right kind of value.

Do you have an idea for how Pulp can deliver better on our goals? Let us
know via twitter @pulpproj.
