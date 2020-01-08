---
layout: post
title:  "Bizarre Late Night Thoughts"
date:   2020-1-7 01:03:00 -0600
categories: 
---

# Bizarre Late Night Thoughts
January 7th, 2020
_NOTE: This post is a stream of consciousness_

<br>
For the majority of my life I think I've been stuck in two states: reflective or anxious. I realize that I have a tendency to not live in the moment at all. I like discussing things in retrospect or talking about what I will do, but I never actually _do_ things in the moment. So what the hell am I doing? Is my life a movie that I'll just nostalgically shed tears to when I'm a 49 year old nearly-retired computer engineer with chronic carpal tunnel? Sounds like a bad plan to me. I've been trying to take initiative to focus on what I'm doing in the present. Everything sounds easy until you realize that knocking out old habits is akin to breaking cob webs with a garden hose: you think it's going to work but only after seeing failure do you realize that you should have listened to your parents in the first place and taken good care of your things/self (yikes!)

<hr style="height:10px; visibility:hidden;" />

Jokes aside, I've been having some really interesting day dreams these days. Sometimes I think about launching a rocket up into space and trying to live in it for a while (Elon Musk would be proud of my ambitions) and other days I'm really just wondering when the next Attack on Titan chapter is going to be released. I try my best to not get too emotionally committed, but I end up pouring in the majority of my brain power to determining how the plot is going to go (don't worry, there are no spoilers here.) 

<hr style="height:10px; visibility:hidden;" />

Rishabh and I have been trying to figure out how to stream new ODs to our implementation of algorithm-B. Currently, there are two really annoying things that are blocking our progress:
* Running/debugging the codebase takes almost 15-20 minutes (sad dayz)
* It seems like some things are being streamed but the tight while loop is not running at all

<hr style="height:10px; visibility:hidden;" />

Nothing can really be done about the first point, but the second one can be tackled to some extent. Basically what I know is that the code is sending the start signal to the binary but it's then not actually reading any of the data that is being streamed to it. I need to debug it, but I have a suspicion that there needs to be some kind of sleep invocation before the data is read through STDOUT. I think the tight loop that reads bytes from the buffer is being skipped over because there is nothing in the moment the code is run. It's a small thing that is hard to miss but I think a simple solution (for now), would be to put a sleep call. I'm going to try that right now.  

<hr style="height:10px; visibility:hidden;" />

Currently this is what the main loop is doing in the Java code:
{% highlight java %}
	Collection<ODMatrix> matrices = entry.getValue();
	ProcessBuilder builder = new ProcessBuilder("./tap","PM_NCTCOG_net.csv","STREAM", "PM_convertertable.csv");
	Process proc = builder.start();
	OutputStream stdin = proc.getOutputStream();
	stdin.write("Start".getBytes());
	stdin.flush();

	String line;
	BufferedReader input = new BufferedReader(new InputStreamReader(proc.getInputStream()));
	while ((line = input.readLine()) != null) {
		System.out.println(line);
	}
	input.close();
	streamODs(matrices, stdin);
{% endhighlight %}

<hr style="height:10px; visibility:hidden;" />


Yeah so the code is still running. Basically what I've done is put in a sleep call right before the tight loop runs checking for things in the buffer. Side note: the Java codebase sucks up all my CPU, memory, and power. I really need a more powerful machine. Hopefully in the future I can afford a nice powerful desktop if I work with things like this. . .

<hr style="height:10px; visibility:hidden;" />


Nope the damn thing still isn't working. OK I figured it out and I realized that I'm a complete knobhead. Basically I was running a loop to output the process and it ended up being that I needed to redirect the output to a file so that I could see what was going on. I'm an IDIOT. I have been extremely absent minded in my thinking all of today. The proper remedy to this is to sleep but I'm not doing that either lol. . .

<hr style="height:10px; visibility:hidden;" />

Ok so I tried that and now the STDOUT is going to the console? Man what the hell is going on. I'll give this a go later, for now I'm gonna take a break.