# The Day We Stopped Losing Context: My Context Engineering Journey

A leadership narrative on turning a field workaround into a reusable operating pattern, from the Dropbear crew in ANZ ISE.

## Prologue: the day progress kept resetting
I'm an engineer in the "Dropbear" crew, part of the ANZ engineering studio within Microsoft's Industry Solutions Engineering (ISE) team. Over 18+ months of working with large language models for code generation, I witnessed the same frustrations that plagued every engineer in the industry: inconsistent code accuracy and style, inability to understand nuanced context, and constantly falling off the context size cliff.

The breaking point came while embedded with one of Australia's largest financial institutions during the early GPT-3.5 era. Their security constraints were unique: access to vetted models was only through their own model gateway via a web portal with a ChatGPT-like UX. No IDE integration. No session persistence initially. We were using GPT-3.5 to bootstrap Python FastAPI applications, sketch low-fidelity architectures, and generate class/function skeletons.

The experience was brittle and frustrating. The model router would routinely hit rate limits mid-session. The portal would go offline for updates as the platform team maintained and fixed the system. When sessions resumed, our conversation history (and the crucial "why" behind our decisions) was gone. We'd restart, re-prompt from scratch, hope to get the thread to roughly the same state, and inevitably hit the context window cliff again. Even when session preservation was introduced, long-running sessions meant hitting context limits fast without any real means to preserve the important aspects.

This is where I first felt the acute need for what I would later learn was called "context engineering." I needed a way to prune and summarize conversation history and externalize this knowledge so I could reuse it as required.

## The first shift: treat context as an artifact
Instead of relying on ephemeral chat logs to carry understanding, I started putting more effort into planning with the LLM and externalizing that context. I began breaking plans into smaller, engineer-sized tasks and ensuring these planning documents included implicit knowledge: existing unique code samples, special patterns, domain quirks, and requirements specific to the project.

I treated these documents as handover-grade notes for the next phase of implementation. The plans were still for things engineers would generally think of as smaller units of work (implement a class, refactor a module, write tests) rather than the broad activities that modern coding agents generate.

I would save these plans as external markdown files in the repo, and for each task in the plan, I'd sequentially create a new chat thread with the input being the plan plus any important observations from the previous task. This planning document grew slowly but captured "just enough" context for consistent continuity.

This technique allowed me to produce more accurate code more often. Even when there were issues with generated code, I could recover to a previous checkpoint easily while enriching the document with information about why things went wrong. I was able to do experiments more often and try various prototypes without having to start over each time.

## A small implementation that unlocked continuity
To make this practical, I refined the approach into a lightweight habit: a single markdown file per objective that traveled with the work. I nicknamed it the "breadcrumb." It wasn't a transcript of everything that happened; it was a thin, durable thread capturing:
- What changed
- Why it changed  
- What's next

With that simple structure, I could restart any time (new session, new day, new teammate) and keep moving without re-litigating old decisions. The first effect was modest but undeniable: fewer resets, faster iteration on medium-sized tasks, and cleaner reviews.

Reuse wasn't just code anymore; it was context.

## The ecosystem catches up, and the same gap persists
By early 2025, the AI landscape had evolved dramatically. Stronger models like Claude Sonnet 3.7 arrived alongside a wave of sophisticated coding agents. These tools could plan multi-step workflows and execute with impressive autonomy. My team began experimenting with these coding agents and prompts that instructed them to carry out multi-step plans with human supervision.

But the fundamental issue remained: without an external thread of shared understanding, agents would contradict prior choices and humans would lose time re-establishing intent. The same broader problem of "context misalignment" that I'd experienced with GPT-3.5 still plagued the agentic coding experience:

- **Inconsistent Implementation**: Without access to full context and reasoning behind previous decisions, AI suggestions contradicted established patterns or architectural choices
- **Knowledge Silos**: Critical decisions and their rationale remained trapped in ephemeral conversations or only in my mind, making it difficult for team members and agents to understand the "why" behind implementation choices  
- **Progress Fragmentation**: Development became a series of disconnected interactions rather than a coherent journey

I realized the planning documents needed to evolve for this new environment of more capable models, larger context windows, and autonomous agents.

## From individual practice to team operating pattern
I wanted these context documents to be first-class citizens within our code repositories, not throwaway assets used for one coding session. I developed a workflow that instructed agents to continuously externalize memory to what I formalized as a "breadcrumb file."

The breadcrumb wasn't just a place for conversation history. It captured "just enough" information gathered through analyzing code, reading documentation, and incorporating human feedback. It represented a scratch pad where the engineer and agent could "align" on their understanding of the task, a way to give agents hindsight through continuously pruned and summarized context.

One powerful side effect: each logical coding session resulted in a breadcrumb file that became incredibly useful for pull request reviews. The document that helped achieve alignment between engineer and agent was now helping achieve the same alignment with reviewers.

I began sharing this approach across teams within ISE and beyond. The response was encouraging. Other engineers were facing the same context debt challenges, and the structured approach was helping them too.

As word spread, I had the opportunity to work with the newly formed Cassowary crew to help build context engineering capabilities across our ANZ studio. We used the breadcrumb protocol as our training ground, applying it to real code generation challenges while refining the approach together. The collaboration was productive. We were learning what worked at team scale while proving the concept could transfer beyond individual practice.

## Testing at scale: the Azure Service Insights collaboration
A pivotal moment came when I got the opportunity to work directly with an internal Azure Service Insights (ASI) team, part of the Azure Kubernetes Service (AKS) ecosystem. They were working on a large, brownfield codebase with years of implicit domain knowledge buried deep in code. The challenge was ambitious: deliver a new feature while simultaneously carrying out a major refactor to simplify the component model and cut down technical debt.

This was the perfect test case for context engineering at scale. Brownfield environments surface all the hidden knowledge that makes or breaks AI-assisted development. This was exactly what we needed to prove the breadcrumb approach could handle complex, real-world production systems, not just greenfield prototypes.

During this time, GitHub Copilot didn't yet have extensive support for prompt files, chat modes, or MCP tools, which made implementing context engineering workflows challenging. However, this constraint actually made the structured breadcrumb approach even more valuable. It worked with any editor, any model, any agent.

As I worked with the ASI team to apply context engineering practices to their brownfield environment, Team "Taipan" within our ANZ region was already carrying out Hyper Velocity Engineering (HVE) experiments. The ASI team's partnership with Taipan created an opportunity to further refine the workflow, with results from our early experimentation informing how both teams adapted the practices for complex, real-world systems with deep, implicit domain knowledge.

The collaboration was successful. The ASI and Taipan teams were able to deliver a new feature while also simultaneously carrying out a major refactor of the codebase to simplify the component model and cut down on technical debt. This proved that AI-assisted workflows, when grounded in proper context engineering, could tackle complex, real-world projects without losing coherence.

In parallel, I was working with Bill Berry and Katrien De Graeve from the edge-ai-accelerator team to understand what made context management effective in GitHub Copilot workflows. The timing was perfect. They were running extensive experiments, and I could contribute insights from the breadcrumb approach while learning from their work. When I had the chance to present our findings to Harald Kirschner from the VS Code product team, the conversation focused on how task-based planning and memory externalization could improve AI-assisted development. It was validating to later see many of these patterns become native features in GitHub Copilot and other coding agents.

## Independent evaluation
To validate what we were observing anecdotally, I connected with two data scientists from ISE based in the Netherlands (Ihar Shulhan and Leonard Herold) who conducted an extensive evaluation of the breadcrumb workflow.

Their key finding was revelatory: the benefits didn't come from simply "feeding more text" to models. They came from creating structured dialogue that builds shared understanding. The breadcrumb files became living documents capturing not just *what* was built, but *why*.

The experiment revealed quantifiable benefits:
- **Enhanced Developer Autonomy**: Agents demonstrated significantly improved ability to progress through complex tasks with minimal intervention
- **Accelerated Development Velocity**: The phased planning approach enabled faster development cycles, particularly for medium to large implementation tasks
- **Robust Context Preservation**: Breadcrumbs effectively maintained critical context across sessions and task switches, eliminating the need to restart from scratch

In other words, context engineering (not any particular tool) was the driver. The breadcrumb was simply a pragmatic way to practice it systematically. The results of this evaluation and the breadcrumb protocol itself have now been contributed to the ISE AI Garden repository for continued use and experimentation.

## What we learned
Several key insights emerged from developing, scaling, and validating context engineering practices:

- **Context debt is real and compounds with scale**: Bigger context windows don't fix alignment drift on their own. Structure and externalization are essential.
- **Small, disciplined artifacts win**: A single, tightly pruned file per objective beats sprawling documentation or ephemeral chat logs.
- **Practices beat platforms**: The protocol works with any editor, any model, any agent. It's tool-agnostic by design.
- **Reuse grows when understanding travels**: Code reuse follows context reuse, not the other way around. Share the "why" and the "what" becomes more valuable.
- **Human expertise remains essential**: Context engineering requires domain understanding, pattern recognition, and judgment that only experienced engineers can provide.

## Where we go next
Context engineering has emerged as a critical industry pattern, and our work has helped validate and refine practical approaches to implementing it. For teams looking to adopt similar practices, here's what we've learned works:

- **Treat context as a first-class artifact**: Keep a single lightweight file per objective in your repo and link it in PRs. This pattern has proven effective across different teams and codebases
- **Start simple**: What changed, why it changed, what's next—this minimal structure actually works better than complex frameworks
- **Make it travel with the work**: The context document should live in the repo, not in external tools or ephemeral chats
- **Focus on the "why"**: Capture decisions and rationale, not just implementation details

Our breadcrumb protocol represents one concrete realization of context engineering principles. As the industry continues to develop this discipline, our contribution lies in demonstrating that these practices work at scale and providing a proven implementation pattern that other teams can adapt to their own contexts.

## Closing
What started as a field workaround for brittle AI sessions has helped validate and realize an emerging industry pattern. Context engineering was gaining traction across the industry, but our work provided concrete proof that it could scale across teams and deliver measurable results.

The breadcrumb protocol is one practical realization of context engineering principles, but the broader discipline is bigger: in the age of AI-assisted development, the most powerful skill isn't prompting the machine—it's scaffolding shared understanding between human expertise and machine capability.

Our experience has shown that effective AI-assisted development requires more than better models or tools—it needs structured practices that preserve context and enable continuity. The patterns we've validated can help other teams avoid the same context debt challenges we faced.

Reuse isn't just code; it's context. And context engineering is how we make that reuse systematic, transferable, and valuable at scale. Our contribution has been proving it works.

---

## Special Thanks

This work wouldn't have been possible without the collaboration and insights from many talented colleagues:

- **Bill Berry and Katrien De Graeve** from the Edge-AI-Accelerator team for their invaluable collaboration on GitHub Copilot workflows and context management principles
- **Harald Kirschner** from the Microsoft VS Code product team for engaging with our findings and helping validate the direction of our research
- **Ihar Shulhan and Leonard Herold**, data scientists from ISE Netherlands, for conducting the rigorous evaluation that quantified the effectiveness of the breadcrumb workflow
- **Jordan Knight** for his research on task-based planning and tracking work that I incorporated into the breadcrumb protocol design
- **The Cassowary crew** for being early adopters and helping refine the breadcrumb protocol through real-world application
- **Team Taipan** for their partnership with the ASI team in scaling context engineering practices to complex brownfield codebases and their expertise during the Hyper Velocity Engineering experiments
- **The Azure Service Insights (ASI) team** for trusting us to test these approaches on their complex, production brownfield codebase

Their expertise, feedback, and willingness to experiment with new approaches were essential to developing and validating context engineering as a practical discipline.

---

## Reference: Microsoft Teams and Initiatives

| Term | Description |
|------|-------------|
| **ISE** | Industry Solutions Engineering - Microsoft's customer-facing engineering organization that works directly with enterprise customers |
| **ANZ Studio** | Australia and New Zealand engineering studio within ISE - a regional engineering unit comprising multiple specialized teams |
| **Team Dropbear** | One of three engineering teams that make up the ANZ ISE studio (author's team) |
| **Team Cassowary** | One of three engineering teams that make up the ANZ ISE studio |
| **Team Taipan** | One of three engineering teams that make up the ANZ ISE studio |
| **Edge-AI-Accelerator team** | Microsoft ISE team focused on AI acceleration and tooling research for edge devices |
| **ASI** | Azure Service Insights - monitoring and observability service for Azure Kubernetes Service |
| **AKS** | Azure Kubernetes Service - Microsoft's managed Kubernetes offering |
| **HVE** | Hyper Velocity Engineering - Microsoft initiative focused on accelerating software development through AI-assisted workflows |
| **ISE AI Garden** | Internal repository within ISE for sharing AI tools, patterns, and best practices across teams |
