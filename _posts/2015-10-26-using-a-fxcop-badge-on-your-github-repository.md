---
published: false
title: Using a FxCop badge on your GitHub repository
---

The use of badges on GitHub repositories helps us to promote good pratices about our code. Some amazing services like the [Shileds.io](http://shields.io) can generate almost all badges you can imagine, but what about those badges where there is no such online service to perform this job? One common case is when you program with C# and want some FxCop badge but there is no online service to run FxCop.

### BadgesSharp
To fill this gap I created the BadgesSharp service: [http://badgessharp.apphb.com](http://badgessharp.apphb.com). BadgesSharp is a free service to generate badges that need some kind of input and processing before you can display them on GitHub repositories.

In the case of FxCop, we need to run it against our .NET code and send the result report to BadgesSharp and then the service will generate the FxCop badge.

Here is a small tutorial on how to get the FxCop badge in your GitHub repo:</p>

### Install FxCop
If you don’t have it yet, [download](https://www.microsoft.com/en-us/download/details.aspx?id=6544) and install FxCop.

### Run FxCop

```
"C:\Program Files (x86)\Microsoft Fxcop 10.0\FxCopCmd.exe" /project:[Your FxCop file].FxCop /out:fxcop-report.xml
```
The report will be saved to fxcop-report.xml

### Generate FxCop badge
Download [BadgesSharpCmd](https://github.com/giacomelli/BadgesSharp/releases) and run it:</p>

```
BadgesSharpCmd -o [your GitHub username] -r [your GitHub repository] -a %GITHUB_REPO_TOKEN% -b FxCop -c fxcop-report.xml
```

Note: you will need a GitHub personal token: [https://github.com/settings/tokens](https://github.com/settings/tokens).

More info at:[https://badgessharp.apphb.com/Docs/GettingStarted](https://badgessharp.apphb.com/Docs/GettingStarted)

### Show the badge at your GitHub repository
Edit your readme.md and add the line below:

```
![FxCop](https://badgessharp.apphb.com/badges/:owner/:repo/FxCop)
```
Sample badges: 

<img src="https://badgessharp.apphb.com/badges/giacomelli/BadgesSharp/FxCop" /> <img src="https://badgessharp.apphb.com/badges/giacomelli/SampleProject/FxCop" />

<h2>How use it on your Continuous Integration?</h2>
<p>Probably you’re using some continuous integration service, below are some samples:</p>
<h2>AppVeyor</h2>
<p>Add to your AppVeyor.yml file:</p>
<pre>
after_build:
        - cmd: >
        "C:\Program Files (x86)\Microsoft Fxcop 10.0\FxCopCmd.exe" /project:[Your FxCop file].FxCop /out:fxcop-report.xml

        BadgesSharpCmd -o [your GitHub username] -r [your GitHub repository] -a %GITHUB_REPO_TOKEN% -b FxCop -c fxcop-report.xml
</pre>
<h2>TeamCity</h2>
<p>Add the ‘FxCop’ step to your configuration (probably you already have it);<br />
Add a final ‘Command Line’ step to your configuration:</p>
<ul>
<li>Execute step: Even if some of the previous steps failed</li>
<li>Run: Custom script<br />
Custom script:</p>
<pre>BadgesSharpCmd -o [your GitHub username] -r [your GitHub repository] -a %GITHUB_REPO_TOKEN% -b FxCop -c "%system.teamcity.build.tempDir%\fxcop-output-*\fxcop-result.xml"</pre>
</li>
</ul>
<h2>Conclusion</h2>
<p>That’s it! Now you have a FxCop badge to show on your GitHub repository.</p>
<p>BadgesSharp support others badges too: StyleCop, DupFinder and Plato.</p>
<p>If you like it, take a look on GitHub repository: <a href="https://github.com/giacomelli/BadgesSharp" target="_blank">https://github.com/giacomelli/BadgesSharp</a>.</p>

