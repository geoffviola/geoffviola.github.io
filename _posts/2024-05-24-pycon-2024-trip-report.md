# Introduction

Pycon 2024 took place May 15-23 in Pittsburgh, PA. I went to the Saturday and Sunday days May 18-19\. There were many tracks and open spaces happening simultaneously. Open spaces were round tables like birds of a feather where people discussed arbitrary topics. There was even a track in Spanish. I went to mostly lectures on Saturday. Then, I went to mostly open sessions on Sunday.

![Outside](/assets/images/2024/pycon2024_outside_small.jpg "Outside the convention center")

# Memorable Lectures

## Sim Willison Keynote

This talk was mostly about large language models (LLMs). He went a little into copyright issues in the training data. Specifically, he mentioned that Meta's LLaMa model had some copyright issues. 4.5% of LLaMa's training dataset Sampling was books according to [LLaMa: Open and Efficient Foundation Language Models](https://arxiv.org/pdf/2302.13971). An author, Sarah Silverman, has [sued](https://www.theverge.com/2023/7/9/23788741/sarah-silverman-openai-meta-chatgpt-llama-copyright-infringement-chatbots-artificial-intelligence-ai) Meta and OpenAI for that. It wasn't mentioned in the talk, but there was a similar case with the [New York Times sueing OpenAI and Microsoft](https://www.nytimes.com/2023/12/27/business/media/new-york-times-open-ai-microsoft-lawsuit.html). OpenAI's CTO, Mira Murati, mentioned that they use ["publicly available videos"](https://www.reddit.com/r/ChatGPT/comments/1bfa7s3/openai_cto_mira_murati_confirms_that_the_video/). But it is against [YouTube's policy to train on their videos](https://www.bloomberg.com/news/articles/2024-04-04/youtube-says-openai-training-sora-with-its-videos-would-break-the-rules).

Another thing that he mentioned is that LLMs don't have a great way to objectively evaluate them. He says that the best way is to qualitatively compare them. He jokingly calls it vibes. There's a tool called [lmsys](https://chat.lmsys.org/) that does a side-by-side comparison of two different models with the same prompt.

## Jacob Lapenna: Open Source Industrial Control: Turning 2,800 Tons of Metal with Python and Flask

This talk covered some work managing a large piece of power generating infrastructure. The speaker is an electrical engineer. His project improved the workflow of maintaining what is a giant DC motor. Overall, it was very interesting to see someone putting together mechanical, electrical, and software skills.

# Notes from Open Sessions

![OpenSessions](/assets/images/2024/pycon2024_open_session_wide.jpg "A picture of some of the open sessions"){:width="500"}

## Dev Ex

There was talk about shadow ops - developers building their own infrastructure with little involvement of IT. Someone mentioned using [localstack](https://www.localstack.cloud/) to improve AWS testing.

## Ricing

Some folks showed off their desktop environment and their tools. There were a lot of vim enthusiasts. One member mentioned using a tool called [btop](https://github.com/aristocratos/btop). Someone showed off a song with [bespoke synth](https://www.bespokesynth.com/).

## AI Tools

1. [cursor](https://cursor.sh/): an IDEA where LLM is at the main feature.
2. [HASH](https://hash.ai/): an AI project that [Joel Spolsky](https://www.joelonsoftware.com/2020/06/18/hash-a-free-online-platform-for-modeling-the-world/) supports
3. [notion](https://www.notion.so/): AI notebook [arc](https://www.helloarc.ai/): an AI bot that tracks investments
4. [quivr](https://github.com/QuivrHQ/quivr): a chatbot trained on custom user data
5. [ShellGPT](https://github.com/TheR1D/shell_gpt): A command line tool with LLM integration
6. [askmarvin](https://www.askmarvin.ai/): a toolkit for building natural language interfaces
7. [descript](https://www.descript.com/): a tool that allows editing podcast audio via text to produce a new audio track.
8. [VOCALOID6](https://www.vocaloid.com/en/): AI generated singing.
9. [BandLab](https://www.bandlab.com/?lang=en): music producing software

## Rust

1. [Online Intro Rust Book](https://doc.rust-lang.org/book/)
2. [rustlings](https://github.com/rust-lang/rustlings): small rust exercises
3. [Learn Rust With Entirely Too Many Linked Lists](https://rust-unofficial.github.io/too-many-lists/)
4. [Rust by Example](https://doc.rust-lang.org/rust-by-example/)
5. [Evcxr](https://github.com/evcxr/evcxr): a rust interpreter that may be used in Jupyter

# Talks to watch

- Anthony Shaw: Unlocking the Parallel Universe: Subinterpreters and Free-Threading in Python 3.13\. 19 May, 02:30 PM - 03:00 PM Track 1 (Ballroom A)
- Jay Miller: Keynote. 7 May, 09:45 AM - 10:30 AM Main Stage
- Reuven M. Lerner: Times and dates in Pandas. Fri 17 May, 2024 - 12:30 PM-01:15 PM Track 2 (Ballroom BC)
- Jules Poon and Ken Jin Ooi: How two undergrads from the other side of the planet are speeding up your future code. 19 May, 01:45 PM - 02:15 PM Track 1 (Ballroom A)
- Kate Chapman: Keynote. 19 May, 09:00 AM - 09:40 AM Main Stage
- Kevin Kho and Kevin Kho: Speed is Not All You Need for Data Processing. 18 May, 01:45 PM - 02:15 PM Track 4 (Hall C)
- Neeraj Pandey and Manoj Pandey: Visual Data Storytelling with Blender and Python. 18 May, 10:30 AM - 11:00 AM Track 1 (Ballroom A)
- Sebastián Flores: Python and Data Storytelling to the Rescue: Let's Avoid More Deaths by PowerPoint. 18 May, 12:00 PM - 12:30 PM Track 1 (Ballroom A)
- Sumana Harihareswara: Keynote. 19 May, 03:15 PM - 03:55 PM Main Stage
