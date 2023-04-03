---
layout: post
title:  "Unix Time and the 2038 Problem"
date:   2023-04-02 15:55:42 -0400
categories: Tech Rambles
---

Imagine: it's 1999 and you're working as a leader of a software engineering team responsible for testing the software at a large tech company, such as Microsoft. You've just been handed an assignment to analyze and mitigate the risks of the Y2K problem.

What was the Y2K problem? Y2K stands for "year 2000". At its core, the issue was one of data storage. Many programs in the 80s and 90s would store information about the current year using a two-digit format. For example, the date March 12, 1987 might simply be represented in memory as "12-03-87". Storing information in this way seems innocent, until we start to consider the ambiguity that can arise as a result of doing so. Consider what might happen if we wanted to represent the date March 12, 2087 using the same format. Well, that date would have precisely the same representation, "12-03-87" as the first example. This ambiguity is the Y2K problem.

The concern was that many programs might store data in this ambiguous format or contain logic which only looks at the last two digits of the year to make a decision. This was especially a concern for financial institutions, which might have differing business logic depending on the current time. If, for some reason, this business logic was implemented through comparisons made on just the last two digits of the year, then that logic was likely to suddenly stop functioning properly once the year 2000 hit.

It might not seem like such a huge issue looking back on it, but the Y2K problem was taken very seriously by large tech companies. I spent one evening looking at many web archives from 1999, like this one: https://web.archive.org/web/19990508062733/http://microsoft.com/y2k/.

Microsoft's website had a page dedicated to the Y2K problem and the measures they were taking to addressing it. I find it comical that there was even a "Year 200 Resource Center CD" created to educate people about Y2K readiness.

What does this have to do with Unix Time?

One might think that after the Y2K fiasco, the software development industry would have learned frmo its mistakes and that something like this would be unlikely to happen again. You'd be wrong.

Unix time, sometimes referred to as epoch time, is measured as the number of seconds since January 1st, 1970 at UTC. Again, there's nothing inherently wrong with this way of measuring time, even if it is a bit arbitrary to decide that 1970 is where things begin. The problem is, on most systems, Unix time is represented as a signed 32 bit integer (uint32). The maximum value that such an integer can hold is precisely 2^(31) - 1 = 2,147,485,547. This means that we can only measure 2,147,485,547 seconds out from January 1st, 1970, which happens to be January 19th, 2038 at 03:14:07 UTC. After that time, the integer will overflow and suddenly systems which rely on these timestamps will think that it is December 13th, 1901 at 20:45:52 UTC. You can see why this is problematic.

But, how much of an issue is this really? Do modern systems still rely on a mode of storing time which has an expiration date sometime less than 15 years from now?

The answer is, yes, they very much do. Use of unix time is almost ubiquitous in many different applications, including but not limited to:

- File systems
- Databases
- Embedded systems

In fact, I encountered a real-life example which made me want to write this article in the first place. The following excerpt is taken from the open source MySQL code base:

{% highlight cpp %}
if (tries > max_tries) {
  /*
    If the time has got past epoch, we need to shut this server down.
    We do this by making sure every command is a shutdown and we
    have enough privileges to shut the server down
    TODO: remove this when we have full 64 bit my_time_t support
  */
  LogErr(ERROR_LEVEL, ER_UNSUPPORTED_DATE);
  ulong master_access = thd->security_context()->master_access();
  thd->security_context()->set_master_access(master_access | SHUTDOWN_ACL);
  error = true;
  kill_mysql();
}
{% endhighlight %}

In its current state, MySQL will not run after 2038. I suspect this is the case for many different pieces of software currently in existence.

And although I have no doubt that MySQL will be extended to eventually support a larger time format, which will solve the problem for a great number of years, I know that there will be software out there which will be forgotten. In 2038, I suspect a lot of old software will inexplicably break or suddenly exhibit random bugs.

Because Unix time is often just represented as an integer, rather than something like a DateTime type, it may be difficult to find all uses of Unix time in a given program, since you can't simply search for all variables that have a DateTime type. It's likely some instances will be missed, and curious bugs will slip through the cracks.

In other cases, such as embedded systems, it may be completely impossible to patch the software. Such devices will experience a coordinated demise, likely to never function quite as intended again.

In the end, I'm sure teams of software engineers will execute thorough testing to ensure the safety of supported software products, making the transition into 2038 as smooth as possible. Still, it's interesting to think about the effort and risk it will take to get there. And it's even more interesting to think about how we've made the same mistake twice.
