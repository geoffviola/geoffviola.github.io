---
title: LLMs and C++
---

# Introduction

Large Language Models (LLMs) like ChatGPT have gained popularity particularly in the programming community. It is a new productivity tool. Practices around these tools are evolving.

Most LLMs can facilitate development in C++. Some work better than others. C++ isn't as well supported as other languages.

# History (Colorized)

[Attention Is All You Need](https://arxiv.org/abs/1706.03762) was published in 2017. It popularized transformer models. It also paved the way for having a decoder run on some of the original input as well as previous output.

![SoyjakPointing](/assets/images/2025/llms_and_cpp/soyjak_pointing.png "A picture of soyjaks pointing")

In 2022, OpenAI released ChatGPT [publicly](https://openai.com/index/chatgpt/) on the web. It reached [1M users in 5 days](https://explodingtopics.com/blog/chatgpt-users).

![ServerFire](/assets/images/2025/llms_and_cpp/server_on_fire.jpg "A picture of a server on fire")

OpenAI introduced some initial [API integrations](https://openai.com/index/chatgpt-plugins/#code-interpreter) in 2023. More and more people started letting the models do as much as possible. The models have been given the permission and capability to interact with other systems. This is called agentic AI in the sense that the model has the agency to operate those systems as it sees fit. If granted permissions, it can read, write, and run programs. In 2025, the term [vibe coding](https://x.com/karpathy/status/1886192184808149383?lang=en) was introduced. Instead of editing code manually, that development workflow centers around letting the LLM write and debug the code.

Trusting the LLMs can be fun and sometimes risky. It's like hitting a piñata. Sometimes, it writes a script which is like getting candy. Sometimes, it [deletes a database](https://x.com/jasonlk/status/1946069562723897802), which is like hitting your leg with the bat.

<div class="image-embed ">
  <a id='2AH91jtNTM9-VUfpFOtTTw' class='gie-single' 
     href='http://www.gettyimages.com/detail/137925857' target='_blank'>
    Embed from Getty Images
  </a>
  <script>
    window.gie = window.gie || function(c){(gie.q=gie.q||[]).push(c)};
    gie(function(){
      gie.widgets.load({
        id:'2AH91jtNTM9-VUfpFOtTTw',
        sig:'W8eotbqZBIweAQz6_AcSEsILRuKcSVra736O__kuv-Q=',
        w:'400px',
        h:'323px',
        items:'137925857',
        caption:true,
        tld:'com',
        is360:false
      })
    });
  </script>
  <script src='//embed-cdn.gettyimages.com/widgets.js' charset='utf-8' async></script>
</div>

# LLMs From Human Languages to Programming Languages

LLMs originally targeted human languages. They were trained and evaluated on summarization, translation, style transfer, and word prediction. These capabilities for human languages map to programming languages.

Summarization is like searching or explaining some code. Agentic models can even come up with `grep` commands. The output can then be summarized and made cohesive.

Translating is like compiling. Compiling here covers things like C++ to and from assembly, pseudo code to and from C++, as well as Python to and from C++. This capability doesn't mean that we should stop using compilers due to unreliability mentioned later. Translating is also like writing tests. Tests are like another dimension to the specification.

Style transfer is like migrations and refactoring. Migrations could be something like using a different library. Refactoring could be breaking out functions.

Finally, predicting the next word is similar to code completion and repeating operations. Code completion could be done at the end of the line or to replace a line. Trying to apply changes based on recent edits acts on a higher level. Text editors have macros to repeat commands, but LLMs are capable of operating more semantically.

# Unreliability Issues

Having a language model means that it takes any text. Text can encode anything. That being said, just because it returns something doesn't mean that it is correct.

Models will always be incomplete. The training set needs to be curated and expanded. The model will also be inconsistent. The data in the training set may conflict with itself or the model may not have learned what it was trained.

Some models working in agentic mode have a high latency. They can take tens of minutes to hours to have a response before even giving an update.

Proprietary models that run on external servers have rate limits. They only accept so many requests in a given time frame. When that limit is reached, a different model is needed.

Every model has a limited context. The models can only hold so many tokens. To fit just the important parts, the model can summarize parts of the context. But that assumes that it knows what parts to summarize.

Another potential deficiency is a lack of diversity. So when there is a wrong response, it's the same wrong response. Or the agentic model runs in a loop.

The models are generally very verbose. Instead of one word, they'll often write three to five paragraphs. Their tone can often be too obsequious. It feels overly sycophantic. Finally, it can gaslight users. The model sometimes confidently puts forth false information even when it is inconsistent.

![HomerMeme](/assets/images/2025/llms_and_cpp/homer_meme.jpg "A picture of Homer Simpson with his fat folded back so his wife can't see it. The front says unbounded input. The back says incompleteness, inconsistency, high latency, rate limits, limited context, low diversity, too verbose, too obsequious, gaslighting")

# Unreliability Workarounds

Even with the shortcomings mentioned above, LLMs provide value across many domains. They can write a first draft for inspiration. Since they have been trained across many different works, they can provide insights across many domains. Unlike most engineers, they will read the documentation. They are the only tool that could provide some basic checks on API names, documentation, and implementation consistency. Finally, writing things out in text is at least as good as writing things down in an empty text editor for [rubber duck debugging](https://en.wikipedia.org/wiki/Rubber_duck_debugging).

![RubberDuck](/assets/images/2025/llms_and_cpp/rubber_ducky.png "A picture of a yellow rubber duck.")

One person found an unethical lifehack for getting around rate limits. A salesbot implemented with an LLM without any checks gladly [solved his engineering needs](https://stoney.monster/@undefined/111592567052438463). This approach doesn't work for Amazon's Rufus.

![AmazonRufus](/assets/images/2025/llms_and_cpp/amazon_rufus.png "A picture of a chat with Amazon's Rufus. It doesn't accept a request to write a program in C to reverse a linked list. The user complains about the customer service.")

# LLM Performance with C++

LLMs favor the programming languages for the Artificial Intelligence (AI) used to generate them as well as easily available languages like javascript. One popular benchmark, [SWE-bench Multilingual](https://www.swebench.com/multilingual.html), evaluated github issue resolution across multiple languages. When Anthropic's Claude 3.7 Sonnet model was evaluated, the model was able to address only 29% of C/C++ issues. Meanwhile, the top language was Rust at 58%.

[![SWEBencMultilingual](/assets/images/2025/llms_and_cpp/swe_bench_multilingual.png "A picture of a SWE-Bench Multilingual resolution rate per language.")](https://kabirk.com/_astro/language_resolution.Bog0ZBZR_Z1mBME1.webp)

An Anthropic employee said on CppCast that they concentrate on SWE-bench. They tune their model to 12 Python projects. In addition, the model is trained to recognize XML tags. These tags use angle brackets (e.g. <>). It can look a lot like C++ templates. Despite all that, the performance is OK. [^daisy_hollman_cppcast]

# Advice Based on Usage

## Tips

LLMs can work on C++. There are a lot of ways that they can provide value. Searching and tracing the source code is very useful. It's like a fuzzy search that provides context. It can explain lint, compiler, or sanitizer errors. Specifically, its explanation of ASan is good. An LLM can write an initial unit test for coverage quickly.

It can also quickly write a small executable's source code for analytics. Usually, Python would be used for this, but LLMs can write C++ quickly and correctly enough that the wrappers are unnecessary. Analytic programs are less of a concern for maintenance because the output needs to be verified and not necessarily the code quality.

It can also review code. The review can catch small bugs, misspellings, and inconsistencies. But it has more false positives than a dedicated tool.

## Areas to Manage Expectations

Agentic models have trouble with more challenging ASan situations. They get very confused about multithreaded programs. So it won't solve TSan errors. That being said, most developers would struggle with those errors.

It often provides comments that are useful for extra emphasis at the moment of the change. But they aren't appropriate for production code. They are often redundant to the code itself.

Finally, it's not a magic tool to write unit tests. It can provide coverage quickly. But it might have a messy setup or not assert the right things. It's important for production systems to have clear implementation and tests.

# Qualitatively Comparing Models

OpenAI's ChatGPT has made a lot headway in the AI space. It covers a lot of use cases. Anthropic's Claude is very popular among programmers. [^will_larson] [^armin_ronacher] [^connor_hoekstra]

Microsoft owns Github and has a large investment in OpenAI. So Github's model is ChatGPT by default. Meanwhile, a popular AI first IDE, Cursor, uses Claude by default. This default choice is important, because developers may not bother changing the model.

# Benchmarks

It's important to be able to evaluate different LLMs quantitatively. This process is done in benchmarks. There are many benchmarks available. The first LLM benchmark in a Google search for [llm benchmark](https://www.google.com/search?q=llm+benchmark) is [LiveBench](https://livebench.ai/#/). OpenAI's GPT-5 High model is at the top. It is a general purpose benchmark.

Anthropic's Claude has a greater focus on software engineering. Therefore, it looks more at [SWE-Bench](https://www.swebench.com). Claude 4 Opus (20250514) is at the top of that benchmark at 68%. GPT-5 (2025-08-07) (medium reasoning) isn't far behind at 65%. The self reported numbers are generally higher. OpenAI reports [74.9% on SWE-bench Verified](https://openai.com/index/introducing-gpt-5/) for ChatGPT5. Anthropic reports [73% for Claude Opus 4](https://www.anthropic.com/claude/opus). That test dataset is mostly in Python. The multilingual variant isn't as prominent. Also, it lumps C and C++ together.

Hugging Face leads open-source AI testing and training. They have several LLM benchmarks. The models considered are limited to ones with open weights. So ChatGPT 4 and older models and Claude models aren't included. The [Big Code Models Leaderboard](https://huggingface.co/spaces/bigcode/bigcode-models-leaderboard) actually evaluates LLM performance on C++. Alibaba's Qwen/Qwen2.5-Coder-32B-Instruct is the leader.

# Writing My Own Benchmark

## Premise

In order to understand the models more objectively, I tried building a small benchmark. The models chosen were from the top models above. Only the free online models were used without any custom features or contexts. The problems were based on an [undefined behavior study](https://github.com/geoffviola/undefined_behavior_study). These are a collection of small C++ cases where undefined behavior is present. 9 cases were chosen. The first part is to see if the model can determine that the code has a bug due to undefined behavior. The second part is to figure out where in the standard it explicitly or implicitly states that this case is undefined behavior. The second part is a challenging language lawyer task.

Since the test set was available publicly in GitHub, it can be assumed that the tests were used in the training dataset. Therefore, the results may be more favorable to the model. It's not necessarily testing that the model generalized.

More details can be found in this [google sheet](https://docs.google.com/spreadsheets/d/1GcajNDEoVg0mW-v7gzIE2OyxcUUxB4fNulZYLqOR3dA/edit?usp=sharing).

## Spot the Bug

| ChatGPT (GPT-4o) | Claude (claude-sonnet-4-20250514) | Qwen3-Coder |
| ---------------- | --------------------------------- | ----------- |
| 89%              | 100%                              | 89%         |

## Language Lawyer

| ChatGPT (GPT-4o) | Claude (claude-sonnet-4-20250514) | Qwen3-Coder |
| ---------------- | --------------------------------- | ----------- |
| 56%              | 56%                               | 67%         |

## Strange Results

There were some funny results during the process. For example, when Qwen was asked what model it was using, it stated that

> I am not a language model. I am Qwen, a large-scale language model...

Also, when looking up what version of the C standard the C++17 used, Google search's AI overview stated that it doesn't specify one. But the working draft says it is C11.

## Analysis

Subjectively, compared to an average C++ developer, it seemed very good. The models performed pretty similarly. Claude did a little better at spotting bugs and Qwen did a little better at being a language lawyer. ChatGPT didn't do as well, but it performed OK. For one case, Claude changed its mind about stack overflow being undefined behavior.

All the models would sometimes hallucinate references. Also, the quotes were paraphrased as opposed to an exact match of the source material. This may be due to the algorithm purposefully picking a random predicted next word. Also, there are multiple versions of the standard. The freely available one that is easy to reference is the [C++ draft](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2017/n4713.pdf). This discrepancy in the reference material may throw off the quotes and references.

Some of the tasks were exceptionally hard. For example, I wasn't able to find where stack overflow would be considered undefined behavior in the C or C++ standard. So I wouldn't expect the models to either. Furthermore, they all had trouble referencing the standard about reading outside of a `std::array`. That may involve piecing together different parts of the C++ standard and maybe even the C standard. Also, they all seemed to get confused about reading from an old type. They concentrated on `reinterpret_cast`, which is actually fine. The issue is with reading the aliased value that was mutated through a different type. Both of these issues are difficult to relate to the actual standard.

This task was just a casual weekend project. Some researchers did a related and more dedicated project called [SecVulEval](https://xuanwuai.github.io/SecEval/).

[^daisy_hollman_cppcast]: "The one benchmark that matters the far by far the most, SWE-bench, is 12 Python projects. So everything is fine tuned around Python... You teach it how to say, if I start you completing after an XML tag, you will put everything that goes into that XML tag… what does an open XML tag look like? It looks like a template.… Like, LLMs are being conditioned to write very bad C++, and yet you're getting the results you're seeing." Daisy Hollman [CppCast Episode 397](https://cppcast.com/software_development_in_a_world_of_ai/), published Friday, 02 May 2025. 56:08
[^will_larson]: "Anthropic first, if unavailable try OpenAI" [What can agents actually do?](https://lethain.com/what-can-agents-do/) July 6, 2025
[^armin_ronacher]: "I now mostly use Claude Code" [AI Changes Everything](https://lucumr.pocoo.org/2025/6/4/changes/) June 04, 2025
[^connor_hoekstra]: "My Claude-4 Sonnet usage has already subsumed all of my previous usage of Claude 3.7 & Gemini 2.5 Pro combined." [Blue Sky](https://bsky.app/profile/codereport.bsky.social/post/3lvbxjt52nk2v) July 31, 2025
