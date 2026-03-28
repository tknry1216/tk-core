# Torvalds Speaks: Future of AI

- **Source**: https://www.youtube.com/watch?v=4WCTGycBceg
- **Duration**: ~9:30

---

## Transcript (corrected)

**(00:00)** Let's go for something that's fun and entirely non-controversial. So, let's talk a little bit about AI and large language models. [laughter] I like you. You're as cynical as I am. That's a good sign. That's a good sign.

**(00:20)** So obviously this is the current hype kid on the block, and if you want to double your salary, add AI to your title. And it's hilarious to watch the self-description of companies — every company on earth has an AI angle to their story as of this year. It is amazing.

**(00:40)** But what I find so interesting is this idea that Gen AI is going to be the end of — you know, insert whatever — end of programmers, the end of authors, the end of movie creators, the end of so many jobs. So you are going to be replaced by an AI model. Finally! [laughter]

**(01:00)** Yeah, no, I don't know. I hate the hype. At the same time, I think AI is very interesting. When I went to university, we were still talking about rule-based expert systems and Bayesian logic and all of this kind of thing, and it was called AI, but it was not in any way intelligent. And I do think that the last few years have been huge. Yeah. But I don't want to be part of the hype, and I will say that the AI revolution has had, even on the kernel level, a few positives.

**(01:54)** For example, a company like Nvidia, who is not exactly famous for being great at interacting with the kernel community, has actually been much more active and been involved in the Linux memory management code, because suddenly they start caring about Linux when they are selling a lot of AI hardware. I mean, it used to be crypto, and it's still obviously GPUs, and it's being used in big servers running Linux.

**(02:27)** So it has actually had a positive impact. But my personal opinion is: let's wait 10 years and see where it actually goes before we make all these crazy announcements of "your job will be gone in 5 years."

**(02:45)** Well, I think we already see lasting and almost irreversible change happening through many of these AI tools. Not on the high Gen AI front, but just in the tools that make our lives better and easier. I keep calling it "autocorrect on steroids" and people are mad at me, but if you use a Gen AI — and hopefully an SLM — tool to help you with basically code completion in your editor, isn't that a huge opportunity?

**(03:15)** So, I'm actually — I mean, I'm one of those people who are very optimistic about AI, and I'm looking forward to the tools to actually find bugs. We have a lot of tooling around the kernel and around any software project obviously, and we use them religiously. But making the tools smarter is not a bad thing, and I to some degree compare it to writing things in assembly — which literally, I started doing with the initial kernel, which was I think about 50% assembly language — and using a compiler and using smarter tools is just the next inevitable step.

**(03:58)** So that's going to happen, but I don't think it's necessarily the gloom and doom that some people say it is, and I definitely don't think it's the promised world that the people who are having their hand out for cash say it is. So, you need to be a bit cynical about this whole hype cycle in the tech industry.

**(04:19)** I hope you all realize that before AI, it was crypto. Before crypto, it was — whatever — it's cloud native. Cloud native. [laughter] There's a loud voice in the front: "That's not hype!" Okay, I mean — the hype, there's always like a grain of reality behind it, but you need to be careful about all the BS around that grain. "You can't say BS." So the "beautiful science" is what he meant. Yes.

**(04:50)** So I look at these tools, and — you said assembler, and then we talk about compilers, then we talk about things like Sparse, a tool that tries to find certain kinds of bugs. We talk about Coccinelle to do code refactoring. We have had for the last 30-plus years a sequence of tools that helped make development better and more robust. And in that lineage of things, I hope there is really cool stuff coming down.

**(05:23)** Yeah, I mean, we have tools that do kernel rewriting with very complicated scripts and pattern recognition and things like that. And that is actually literally why I think AI can be a huge help, because some of these tools are very hard to use because you have to specify things at a low enough level that the natural reaction would be, "Hey, can we make this a bit easier and automate more of it?"

**(05:57)** So, yes. One of the interesting angles that this whole large language model and the training data brings up is the role that data plays in our modern world, where we all talk about open source — about the source code, the algorithms being available — but open data really is kind of the almost more interesting question.

**(06:15)** No, it's not. Well, I mean — [laughter] — it's not to me. And actually, I'd like to clarify that — I mean, the LF obviously has open data projects, and to other people it's more interesting. And that is, I think, the whole — for me, the point of open source is that different people are interested in different things. And I was always interested in the low-level nitty-gritty of how the CPU actually works, which is why I'm working on the kernel still. But yes, you're right that in many situations what is important is the data that you then use to find patterns and generate new interesting information with. But to me, that's not what I tend to do. And there is that saying: "beautiful science in, beautiful science out." Please translate.

**(07:13)** So, you talk about the things that you love doing, and I want to point out it's been more than a decade since you started a project, and the world is kind of getting a little antsy. So where is the next Linus Torvalds project?

**(07:30)** Oh no. I hope it never happens. And I say that because every single project I've started has always started from me being frustrated with other people being incompetent — or money-grubbing, right? So the reason I started doing Linux was that I couldn't afford the real thing, right? And I said, "How hard can it be?" And it turns out it can be pretty hard, because here I am 33 years later and still working on it.

**(08:08)** But I made the same mistake — it's 20 years ago when I said, "Hey, I really don't think source control management is very interesting," and all these people before me, they clearly got it completely wrong. So I need to do my own. How hard can it be? And I'm actually hoping to never be in that situation again — that there will be somebody else who comes and solves my problems.

**(08:35)** And I have to say, I don't have any huge problems. Linux for me solved all the problems I had way back in '90 to maybe '93, and if it wasn't for the fact that others came around and said, "Hey, I need this," I would not have continued. So while my projects start with something that I need, the things that actually keep them going is then the fact that, hey, this is actually useful to other people — because if it's only something for me, it's not really interesting in the long run.

**(09:22)** So unfortunately we're out of time. I had a lot more fun questions. I guess I will have to ask you those in Hong Kong. But for here, for Seattle — thanks everyone. I hope this was fun. [applause]
