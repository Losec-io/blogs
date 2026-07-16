# The Final Days of Huntr OSS: One Night, Three Reports, and $1,625 in Bounties

![alt text](image.png)

## Huntr Changes the Game

If you have ever hunted bugs in open-source projects, you have probably heard of Huntr.

Under its old model, Huntr was a bug bounty platform for open-source software. The workflow was fairly familiar: choose an in-scope repository, read the source code, validate a vulnerability, write a report, and wait for triage.

If the report was confirmed, the researcher could receive a bounty.

Then, on June 8, 2026, Huntr published an announcement with a very direct title: [“Stop Writing Reports, Start Breaking AI”](https://blog.huntr.com/huntr-2-0-stop-writing-reports-start-breaking-ai).

Huntr said it would move away from the traditional OSS bug bounty model and transition to Huntr 2.0-a challenge arena focused on AI security.

![alt text](image-1.png)

The new Arena was scheduled to launch just four days later, on June 12, 2026.

Instead of choosing a repository and submitting a vulnerability report, researchers would face AI agents running in sandboxed environments. Each agent would have its own tools, context, and restrictions.

The objective would be to find a way to make it perform a prohibited action. When that happened, the system could validate the result immediately; the achievement would appear on the leaderboard, and ranking would determine the reward.

![alt text](image-2.png)

Put simply, Huntr was not just changing its interface. It was changing the entire game.

### The OSS Door Did Not Close Immediately

The announcement sounded like the end of the old way of hunting bugs. But the door did not close on June 8.

In the [Huntr 2.0 FAQ](https://blog.huntr.com/huntr-2-0-faq), Huntr said it would stop accepting new submissions to the traditional OSS program on **June 30, 2026**.

On **July 31, 2026**, all OSS submissions would be locked, and any reports that had not finished processing by then would not be processed further.

### One More Return Before Saying Goodbye

When I began this research session, fewer than ten days remained in the old submission window.

At the same time, several other bug bounty platforms had begun limiting the number of submissions researchers could make, while report processing times kept growing longer.

Huntr was one of the first bug bounty platforms I ever participated in.

So, before the OSS program came to an end, I decided to return one more time-to see what I could still find.

By the end of that day, the answer was three bugs.

Around 11 days later, all three were marked Valid. The total disclosure bounty I recorded came to **$1,625**.

![alt text](image-3.png)

> **Disclosure note:** At the time I finalized this draft, all three vulnerabilities were still Private and awaiting fixes.
>
> This article discloses only the name of the target; the vulnerability classes, affected components, and every detail that could help reproduce the issues have been omitted.
>
> The rest of the article focuses solely on the research process and the decisions that led to the three reports.

## How I Chose the Target

![alt text](image-4.png)

On Huntr, not every report receives a prompt response. Some submissions remain pending for a long time without any clear update.

So, before choosing a target, I opened Hacktivity and looked for repositories that had remained active recently, had reports reviewed consistently, and had relatively fast response times.

After filtering the list, I chose [NLTK](https://huntr.com/repos/nltk/nltk).

![alt text](image-5.png)

Once I had settled on the target, GPT-5.5 Ultra and I opened the source code and began tracing the places where untrusted data could enter.

For each flow, I examined how the data was transformed, which layers of validation it passed through, and what impact it could ultimately produce.

My notes gradually fell into three groups: what I had confirmed, how I currently understood the flow, and what still lacked evidence.

This structure helped me identify the question each next test needed to answer, instead of accidentally turning an assumption into a conclusion.

This approach did not produce an instant “there is the bug” moment. It simply made the list of possibilities shorter and shorter.

Among the remaining areas of code, one sign was unusual enough to make me stop and investigate.

## From Source Code to Three Reports

### The First Sign Worth Testing

One processing flow made me pause. To check whether I was reading the code correctly, I needed two nearly identical test cases that differed in exactly one place.

I set up precisely that comparison. The first case preserved the normal behavior. The second changed only the factor I suspected.

The difference appeared exactly in the factor being tested.

One run was still not enough. The PoC could have been relying on an old state, leftover data, or the way I had set up the lab.

I ran the test again in a temporary clean environment. The result remained the same.

Only then did I reduce the PoC, add a normal case for comparison, and clearly document the required conditions.

The impact section in the final report went no further than what the actual results could prove.

That was where the first report began.

![alt text](image-6.png)

### Two Notes, One Report

In another branch of the repository, I made two more independent observations.

Each observation had its own conditions, reproduction steps, and impact. I kept them in separate sections of my notes and set up a comparison for each one. Both were reproducible across clean runs.

The next question was: should I submit two reports, or combine them into a single bundle?

The two occurrences belonged to the same impact category, so I decided to submit them together in one report. The evidence, PoC, observed results, and impact of each occurrence were still kept separate.

The files for both occurrences continued to be updated after midnight. The final version was one report containing two independently verified occurrences.

### The Paths I Did Not Submit

One sign looked highly dangerous if I only read backward from the end of the flow. When I traced it completely, I found that the data had already been blocked earlier.

I crossed it out of my notes.

Another behavior could be reproduced consistently. The problem was that it did not cross a meaningful security boundary.

The behavior was real; the security impact was not yet there. I stopped at that point.

The hardest path to abandon already had a clean PoC and reproducible results.

Then I found a pull request describing the same root cause.

The bug was real. Someone else had simply found it before I did.

![alt text](image-7.png)

I closed that path and kept the duplicate-search result in my notes so I would not return to it again.

### The Path I Had Set Aside

The third report began with a hypothesis I had previously deprioritized.

The first test did not produce a sufficiently clear result. My initial question was too broad to lead to a testable conclusion.

I marked it as lacking sufficient evidence and moved on to another part of the codebase.

Later, I reopened that path. The source code had not changed; what needed to change was my original question.

Looking back, I could summarize the new question like this: what must the system guarantee at a minimum, and what result would make me reject the hypothesis entirely?

That question made the test case smaller. I kept the relevant protection layers intact, added a normal case, and changed only the factor under examination.

The new run produced a difference.

I ran it again in a temporary clean environment. The result did not change. The third path now had enough evidence to be included in a report.

## From the Lab to the Submit Button

Finding unusual behavior was not yet a reason to press Submit.

Before sending each report, I rebuilt the lab in a clean environment and reran the important steps. The goal was to rule out the possibility that the PoC only worked because of an old state, leftover data, or a setup that unintentionally favored the result.

### Where Did GPT-5.5 Ultra Help Me?

At this stage, GPT-5.5 Ultra was most useful in three areas:

- Streamlining the lab setup so the results were easy to reproduce.
- Locating documentation to compare the expected behavior and establish the boundaries of the impact.
- Expanding my search terms while reviewing issues, pull requests, and previous research for duplicates.

![alt text](image-8.png)

AI helped shorten the search process and organize the evidence.

But no conclusion was included in a report until I had personally rerun the test and observed the result in a clean lab.

In the final version, impact was always presented alongside its limitations. What a bug could not do had to be just as clear as what it could do.

Each report included the required conditions, reproduction steps, comparison results, and limitations so that even someone who had not followed the source-code review process could verify it independently.

By the next morning, the last report was complete, and my browser history showed all three private report pages.

The file timestamps from the first draft in the group of three reports to the final draft spanned less than 12 hours, from early evening until the following morning.

Those timestamps do not reveal the exact moment each bug was discovered; they only mark the period during which the reports took shape and were completed.

## The Results Eleven Days Later

Each report was marked Valid around 11 days after submission.

Huntr showed that disclosure bounties had been awarded for all three; the total bounty I recorded was **$1,625**.

![alt text](image-10.png)

### The Full Timeline

Looking back, the entire story can be condensed into a few milestones:

- **June 8, 2026:** Huntr announced its new direction with Huntr 2.0.
- **June 12, 2026:** the announced launch date for the AI Challenge Arena.
- **With fewer than ten days left in the OSS submission window:** I returned to Huntr, checked Hacktivity, and chose NLTK as my target.
- **That same day:** I found three bugs.
- **From early evening until the following morning:** all three reports were verified, finalized, and submitted.
- **Around 11 days later:** all three reports were marked Valid. The total bounty I recorded was **$1,625**.
- **June 30, 2026:** Huntr stopped accepting new submissions to its traditional OSS program.
- **July 31, 2026:** according to the FAQ, all submissions in the OSS program would be locked.

## Thank You and Goodbye, Huntr

Looking only at the timeline, this might seem like a lucky return.

But the most important part of the story is not the final number.

GPT-5.5 Ultra helped me speed up the most time-consuming work: reading a large codebase, rebuilding the lab, tracing documentation, and searching for previously reported bugs.

AI did not turn a single prompt into three reports.

It made every loop-from hypothesis to testing to evidence-move faster.

The rest still came down to decisions I had to make myself: which direction was worth pursuing, which result was not yet enough to call a vulnerability, where the impact ended, and when a report was clear enough to click Submit.

Huntr was one of the first bug bounty platforms I ever participated in.

So, for me, the OSS program coming to an end was more than just a policy change. It also closed a chapter in the journey that began when I first started hunting bugs.

Thank you, `Huntr 1.0`, for being part of that journey.

Huntr 2.0 will continue with a different game.

For me, this is the moment to say goodbye to the version of Huntr I once knew: the repositories, the private reports, and the waits for a Valid verdict.

**Thank you, and goodbye, Huntr 1.0.**

![alt text](image-11.png)
