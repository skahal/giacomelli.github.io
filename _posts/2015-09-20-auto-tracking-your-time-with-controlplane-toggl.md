---
published: false
layout: post
title: Auto tracking your time with ControlPlane + Toggl
---

So, do you want to track the time you spend in some tasks, but always forget to start the time tracking?

I love to used Toggl.com to time tracking and a few days ago I discovered another amazing app to OSX: ControlPlane.

ControlPlane, in few words, is an app that allow you change your desktop configuration (opened apps, notifications, etc) when some trigger happens.

In my specific case I would like to track the time I spend programming in the newest Skahal’s pet project, a Space Invaders Remake or as we like to call: SIR. Well, I wanted to start a “Programming” task on Toggl every time I open Unity3D editor and stop the same task when close the Unity3D.<

I followed steps bellow to get this done:

### 1) Create the startTogglTimeEntry.sh
We will create the follow shell script that uses Toggl api to starts a task:

```
echo 'Starting "Programming" task on toggl.com'
curl -v -u <api-token>:api_token \
    -H "Content-Type: application/json" \
    -d '{"time_entry":{"description":"Programming","tags":[],"pid":<project-id>,"created_with":"curl"}}' \
    -X POST https://www.toggl.com/api/v8/time_entries/start
<project-id><api-token>
```

As you can see we need to replace two things in this script:

* **API-TOKEN**: this should be replaced by your Toggl api token, go to the bottom of [your Toggl profile page](https://www.toggl.com/app/profile) and get it.
* **PROJECT-ID**: this should be replaced by the id of the project where you want to create the "Programming" task. We need to discover de project id, go to [Toggl.com](https://www.toggl.com/app), select your team, then your project, you'll see an address like that in Safari "https://www.toggl.com/app/projects/&lt;TEAM-ID&gt;/edit/&lt;PROJECT-ID&gt;". Use the value of "&lt;PROJECT-ID&gt;" in the script.

Now save the content on file startTogglTimeEntry.sh

### 2) Create the stopTogglTimeEntry.sh

```
taskid=`curl -v -u <api-token>:api_token -X GET https://www.toggl.com/api/v8/time_entries/current | grep -Eo '"id":([0-9])+' | cut -d: -f2`
 
curl -v -u <api-token>:api_token \
    -H "Content-Type: application/json" \
    -X PUT https://www.toggl.com/api/v8/time_entries/$taskid/stop
</api-token></api-token>
```

This script discover the latest task started at Toggl and stop it.

> Note: you should replace the **&lt;PROJECT-ID&gt;** in this script too.

### 3) Download and installl the ControlPlane
Download and install the [ControlPlane](http://www.controlplaneapp.com).

### 4) Configuring the context
The first thing, you must create the context, go to ControlPlane preferences:

<a href="http://diegogiacomelli.com.br/wp-content/uploads/2015/09/ControlPlane_Preferences.png"><img src="{{ site.baseurl }}/assets/ControlPlane_Preferences-199x300.png" alt="ControlPlane" width="199" height="300" class="aligncenter size-medium wp-image-211" /></a></p>
<p>Select tab "Contexts", and add I new context, in my case a called it "Skahal":<br />
<a href="http://diegogiacomelli.com.br/wp-content/uploads/2015/09/ControlPlane_CreateContext.png"><img src="{{ site.baseurl }}/assets/ControlPlane_CreateContext-300x138.png" alt="ControlPlane_CreateContext" width="300" height="138" class="aligncenter size-medium wp-image-217" /></a></p>
<h2>5) Configuring the evidence sources</h2>
<p>Select the tab "Evidences sources", the option "Running Application" should be checked:<br />
<a href="http://diegogiacomelli.com.br/wp-content/uploads/2015/09/ControlPlane_EvidenceSources.png"><img src="{{ site.baseurl }}/assets/ControlPlane_EvidenceSources-300x41.png" alt="ControlPlane_EvidenceSources" width="300" height="41" class="aligncenter size-medium wp-image-216" /></a></p>
<h2>6) Configuring the rules</h2>
<p>Select the tab "Rules", add a new rule to your context, the rule must be of type "Running Application" to Unity 3D:<br />
<a href="http://diegogiacomelli.com.br/wp-content/uploads/2015/09/ControlPlane_Rules.png"><img src="{{ site.baseurl }}/assets/ControlPlane_Rules-300x79.png" alt="ControlPlane_Rules" width="300" height="79" class="aligncenter size-medium wp-image-215" /></a></p>
<h2>7) Configuring the actions</h2>
<p>Select the tab "Actions", we'll create 3 new actions: the first one is a task to open Safari in the Toggl timer task page, this is an optional action, but I like to see the task running (and I can stop/start it manually sometimes). Add a action of type "Open URL" with the address "https://www.toggl.com/app/timer", select "On Arrival" and your context:<br />
<a href="http://diegogiacomelli.com.br/wp-content/uploads/2015/09/ControlPlane_Actions.png"><img src="{{ site.baseurl }}/assets/ControlPlane_Actions-300x28.png" alt="ControlPlane_Actions" width="300" height="28" class="aligncenter size-medium wp-image-214" /></a></p>
<p>The second one is a action to start the task in the Toggl when the context starts. Add a action of type "Shell Script", in the field "Parameter" type the path to your startTogglTimeEntry.sh script, select "On Arrival" and your context:<br />
<a href="http://diegogiacomelli.com.br/wp-content/uploads/2015/09/ControlPlane_StartTogglTask.png"><img src="{{ site.baseurl }}/assets/ControlPlane_StartTogglTask-300x118.png" alt="ControlPlane_StartTogglTask" width="300" height="118" class="aligncenter size-medium wp-image-213" /></a></p>
<p>The third one is a action to stop the task in the Toggl when the context ends. Add a action of type "Shell Script", in the field "Parameter" type the path to your stopTogglTimeEntry.sh script, select "On Arrival" and your context:<br />
<a href="http://diegogiacomelli.com.br/wp-content/uploads/2015/09/ControlPlane_StopTogglTask.png"><img src="{{ site.baseurl }}/assets/ControlPlane_StopTogglTask-300x116.png" alt="ControlPlane_StopTogglTask" width="300" height="116" class="aligncenter size-medium wp-image-212" /></a></p>
<h2>8) Test everything</h2>
<p>Open Unity3D editor, in the almost same time your context must be activated and the Safari must open the Toggl url with "Programming" task started.</p>
<p>Now, close the Unity3D and the "Programming" task must be stopped on Toggl.com.</p>
<p>That's it. ControlPlane is an amazing app and things we can automate with it is nearly infinite!</p>