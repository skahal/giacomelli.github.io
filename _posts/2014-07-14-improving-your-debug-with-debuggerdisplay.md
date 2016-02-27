---
published: false
itle: Improving your debug with DebuggerDisplay
type: post
---


There is an amazing and easy to use custom attribute on .NET Framework stack that I rarely see in use. It helps and improve the way you can debug your code and you just need a line of code to use it: **DebuggerDisplay**.

Maybe you've used it a lot and already love it, in this case just spread the word ;), but if you are a beginner or an experienced .NET developer and don't know DebuggerDisplay, this is the chance to you to improve your debug skills.

### Imagine this scenario:
We have a class called Tweet:

```csharp
public class Tweet
{
    public string Text { get; set; }
    public User User { get; set; }
    public int RetweetsCount  { get; set; }
    public int FavoritesCount  { get; set; }
}
```

You are debugging a list of Tweets, let me say 200 tweets, and all tweets in the debugger view looks like the image below:
![NoDebuggerDisplay.png]({{site.baseurl}}/_posts/NoDebuggerDisplay.png)

<p><a href="http://diegogiacomelli.com.br/wp-content/uploads/2014/07/NoDebuggerDisplay.png"><img src="{{ site.baseurl }}/assets/NoDebuggerDisplay.png" alt="NoDebuggerDisplay" width="139" height="163" class="aligncenter size-full wp-image-108" /></a></p>
<p>It's clear that is not easy to know what tweets are inside that list. Of course you can use breakpoint conditions, trace, logs and many others resources to help the debug process, but DebuggerDisplay is an easier and very cheap solution.</p>
<p>In our scenario, the most important things about the Tweet class are the text, the username and the retweets count. We'll add the DebuggerDisplay attribute to the class:</p>
<pre>
[DebuggerDisplay("{Text} ({User.UserName}) - RTs: {RetweetCount}")]
public class Tweet
{
    public string Text { get; set; }
    public User User { get; set; }
    public int RetweetsCount  { get; set; }
    public int FavoritesCount  { get; set; }
}
</pre>
<p><strong>Now, that "secret" tweet list looks like:</strong><br />
<a href="http://diegogiacomelli.com.br/wp-content/uploads/2014/07/DebuggerDisplayInAction.png"><img src="{{ site.baseurl }}/assets/DebuggerDisplayInAction.png" alt="DebuggerDisplayInAction" class="aligncenter size-full wp-image-108" /></a></p>
<p><a href="http://thecodinglove.com/post/56130208587/when-debugging-is-easier-than-expected"><img src="{{ site.baseurl }}/assets/whenDebugIsEasyThanExpected.gif" alt="whenDebugIsEasyThanExpected" width="300" height="210" class="aligncenter size-medium wp-image-122" /></p>
<div style="text-align:center;font-size:8pt;"><em>When debugging is easier than expected</em></div>
<p></a></p>
<p><em>More information about DebuggerDisplay on official documentation: <a href="http://msdn.microsoft.com/en-us/library/ms228992(v=vs.110).aspx">msdn.microsoft.com/en-us/library/ms228992(v=vs.110).aspx</a></em></p>
