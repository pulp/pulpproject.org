---
id: 1258
title: Langpacks Support added to Pulp 2.9.0
date: 2016-05-05T19:07:26+00:00
author: Brian Bouterse
layout: post
guid: http://www.pulpproject.org/?p=1258
permalink: /2016/05/05/langpacks-support-added-to-pulp-2-9-0/
categories:
  - Uncategorized
---
Pulp 2.9.0 is still in development, but since [langpacks support](https://pulp.plan.io/issues/1367) has been merged, here is a video highlighting this up-and-coming feature.

Langpacks are metadata snippets for RPM repos that allow yum to identify which additional package will provide a specific language support for a package of interest. For example, package gimp-help could have packages gimp-help-en for English or gimp-help-fr for French. Yum has the ability to fetch the langpack for your specific language, but it needs help from the langpacks metadata stored in the repository&#8217;s comps.xml file. Pulp 2.9 will be able to manage that extra metadata in RPM repositories.

Here is a langpacks example snippet:

> <div class="line">
>   <span class="html-tag"><langpacks></span>
> </div>
> 
> <div class="collapsible-content">
>   <div class="line">
>     <span class="html-tag"><match<span class="html-attribute"> <span class="html-attribute-name">install</span>=&#8221;<span class="html-attribute-value">firefox-langpack-%s</span>&#8220;</span><span class="html-attribute"> <span class="html-attribute-name">name</span>=&#8221;<span class="html-attribute-value">firefox</span>&#8220;</span>/></span>
>   </div>
>   
>   <div class="line">
>     <span class="html-tag"><match<span class="html-attribute"> <span class="html-attribute-name">install</span>=&#8221;<span class="html-attribute-value">gimp-help-%s</span>&#8220;</span><span class="html-attribute"> <span class="html-attribute-name">name</span>=&#8221;<span class="html-attribute-value">gimp-help</span>&#8220;</span>/></span>
>   </div>
>   
>   <div class="line">
>     <span class="html-tag"><match<span class="html-attribute"> <span class="html-attribute-name">install</span>=&#8221;<span class="html-attribute-value">libreoffice-langpack-%s</span>&#8220;</span><span class="html-attribute"> <span class="html-attribute-name">name</span>=&#8221;<span class="html-attribute-value">libreoffice-core</span>&#8220;</span>/></span>
>   </div>
>   
>   <div class="line">
>     <span class="html-tag"><match<span class="html-attribute"> <span class="html-attribute-name">install</span>=&#8221;<span class="html-attribute-value">man-pages-%s</span>&#8220;</span><span class="html-attribute"> <span class="html-attribute-name">name</span>=&#8221;<span class="html-attribute-value">man-pages</span>&#8220;</span>/></span>
>   </div>
>   
>   <div class="line">
>     <span class="html-tag"><match<span class="html-attribute"> <span class="html-attribute-name">install</span>=&#8221;<span class="html-attribute-value">mythes-%s</span>&#8220;</span><span class="html-attribute"> <span class="html-attribute-name">name</span>=&#8221;<span class="html-attribute-value">mythes</span>&#8220;</span>/></span>
>   </div>
> </div>
> 
> <div class="line">
>   <span class="html-tag"></langpacks></span>
> </div>

&nbsp;

See the video below for more info on Pulp&#8217;s ability to maintain and manage langpacks on your RPM repositories.