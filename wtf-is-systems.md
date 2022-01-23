# What in the unholy fsck is "systems"? 🔮
> Yuck! I hate this mess of AI/ML, data science and the unnecessary hype around it. I think I'll do systems :)
> – <cite>Students, including me, after learning ML in college</cite>

Sure, you can find the definition on Wikipedia or other Q&A sites on the web but I think they don't fit well in conversations or they just throw too much info at you. I hope to answer this question concisely at the end of this … uhh … whatever this is.

## Let's ask Wikipedia anyways 🙃
Paraphrasing : "The primary distinguishing characteristic of systems programming (SP) when compared to application programming (AP) is that AP aims to produce software which provides services to the user directly.
SP aims to produce software and software platforms which provide services to other software, are performance constrained, or both (e.g. operating systems, game engines, industrial automation, and SaaS applications)."

Hmmm, that looks pretty good. I'll compress this (lossily) to : "Performance constrained software / platforms that server other software". I don't think that's enough yet. This also includes [ML APIs](https://www.perspectiveapi.com/) doesn't it? That may be right but *I don't think* it's what people mean when they say "I do systems" (yes, in this age, it is also possible to *do* systems).

The [Rust book](https://doc.rust-lang.org/book/foreword.html) and [This article about systems in Go](https://golangdocs.com/system-programming-in-go-1) mention that it is "low level programming" that is "closer to the hardware" (similar to the ISA of the computer) and has to "deal with memory management". That seems to be important but I think there's more to it. Also, saying "I do low level programming" sounds ambiguous since these days, you can specialize.

## What does the academic world say? 🧐
Talking about specializations, there's one in our college that says "systems and core computing" (it was a popular one, not as popular as the "machine intelligence and data science" one :P). I don't remember all the courses that were in it but here are a few:
* Advanced Algorithms (This is a core computing one I think.)
* Big Data (Processing and analyzing large data sets. A lot of cool tech. This is the most "systems" thing I have come across.)
* Generic Programming (Core computing prolly)
* Heterogeneous Parallelism (CPU and GPU parallel processing. Algorithms too. This also gives off a strong "systems" vibe.)

I also like [CSRanking's](https://csrankings.org) definition. I think it's what people talk about when they say "systems" (not sure if that's reliable, since I don't know too many people \* cries \*.). Anyways, here it is (a few select ones):
* Computer Architecture (nods head)
* Computer Networks (tilts head)
* Computer Security (squints eyes)
* Databases (nods head)
* Embedded & real-time systems (bangs table)
* Measurement & perf analysis (nods head and sips water)
* Operating systems (jumps on table and hollers)

Hmmm .. looks like there's too much variation. Maybe you should just tell people what specifically you're working on? Fret not, friends. You shall have an answer soon.

## Bits and pieces from the internet 🍬
* [Britannica](https://www.britannica.com/technology/systems-programming) says it involves "data management", developing programs that are part of the operating system, etc.
* [This book](https://g.co/kgs/1dzUSy) says systems software "interfaces directly with the kernel and core system libraries. JavaScript interpreters and the JVM are examples of system software. Although kernel development is part of systems programming, there is also "user-space system-level programming".
* [This answer on SE](https://softwareengineering.stackexchange.com/a/151627) says that systems programming involves "extending and enhancing the features of an OS" and "It mostly involves the Memory Management; I/O operations in the broadest sense of the word like networking, file access and device management; process management (multi tasking, process administration, etc.)". **This sounds like a good way to tell if something is systems or not**.
* [This article explains what systems programming is.](https://hackernoon.com/systems-programming-d5917e41353f) I think it's pretty good.

## Soo … What do you do again? 😪
This is what I shall use when someone asks me what "systems programming" means: 
**Developing performant software that interacts with operating system and other computer hardware closely. It is the base for application software and other software where programmers are the end-users.**. Ehhh… I don't think that's an amazing answer or anything. Just tell them whatever you feel like telling lol.

Although, this sounds like a better answer: 
"It means I like computers more than I like people. \* awkward silence \*. Now stfu while I configure my new terminal emulator to hide cpp warnings. Oh, btw, I use Arch."

P.S. To hell with labels.