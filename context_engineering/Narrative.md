# Throw Away The Vibes: Context Engineering Is All You Need

## Introduction

### Talk Abstract

We've all been there: GitHub Copilot promises to be your coding companion, but instead feels more like that overeager intern who confidently writes brilliant code for the wrong problem. As AI-assisted development tools become ubiquitous, the industry narrative promises revolutionary productivity gains. Yet many practitioners find themselves playing an exhausting game of context whack-a-mole—constantly explaining, re-explaining, and fixing what their AI "partner" confidently got wrong.

Having spent considerable time wrestling with this gap between AI promises and reality, I've discovered that the real challenge isn't prompt engineering—it's context engineering. This talk explores why "vibe coding" is fundamentally broken and introduces the systematic discipline that separates magical AI experiences from expensive disappointments. Through the constraint-context matrix and real examples like the Breadcrumb Protocol, I'll demonstrate why human expertise in scaffolding, steering, and domain understanding isn't just relevant—it's the secret ingredient that makes AI actually work. You'll leave with practical strategies for engineering context systematically and a framework for building sustainable human-AI collaboration that leverages both your skills and the machine's capabilities.

## Content

I recently wrote about a workflow to use with AI assisted engineering here https://dasith.me/2025/04/02/vibe-coding-breadcrumbs/.

After writing it I came across this post https://blog.thepete.net/blog/2025/05/22/why-your-ai-coding-assistant-keeps-doing-it-wrong-and-how-to-fix-it/ and it made it clear to me as to why my previous approach works well by providing implied context.

This is also explained by the unreliable nature of LLMs as described here too: https://verissimo.substack.com/p/verissimo-monthly-may-2025 and many technology leaders are warning against the vibe coding hype. https://www.geekwire.com/2025/reality-check-microsoft-azure-cto-pushes-back-on-ai-vibe-coding-hype-sees-upper-limit-long-term/

This is where the context engineering comes in to play.
- https://rlancemartin.github.io/2025/06/23/context_engineering/
- https://www.philschmid.de/context-engineering
- https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html

But context engineering itself requires deep domain understanding to be effective, as explained here: https://medium.com/@springrod/context-engineering-needs-domain-understanding-b4387e8e4bf8

This post highlights why operator skills still matters. https://ferd.ca/the-gap-through-which-we-praise-the-machine.html

## Key Themes

1. **The Context Engineering Challenge**: The fundamental problem of context misalignment between developers and AI assistants
2. **Why AI-Assisted Development Fails**: Understanding the constraint-context matrix and the gap between AI promises vs. reality
3. **Context Engineering as the Solution**: Moving from prompt engineering to systematic context management
4. **The Enduring Role of Operator Skill**: Why human expertise, scaffolding, and steering remain essential
5. **The Breadcrumb Protocol**: A concrete example of structured workflows that bridge the context gap

## Talk Topic Outline (20 minutes total)

### Opening: The Vibe Coding Problem (3 minutes)
- **The Promise vs Reality**: AI coding tools promise magic, deliver frustration
- **The Core Issue**: Context misalignment between developers and AI assistants
- **Why This Matters**: Transition from "vibe coding" to systematic engineering

### 1. Why AI Coding Fails: The Constraint-Context Matrix (5 minutes)
- **Two Key Dimensions**: Open vs. closed solution spaces, implied vs. provided context
- **AI's Limitations**: Senior-level implementation, junior-level design decisions
- **The Sweet Spot**: When AI works well vs. when it struggles
- **Common Failure Modes**: Poisoning (hallucinations), Distraction (over-focus), Confusion (irrelevant info), Clash (conflicting info)
- **Industry Reality Check**: Technology leaders warning about limitations

### 2. Context Engineering: The Solution Framework (6 minutes)
- **Definition**: "The art and science of filling the context window with just the right information at the right time"
- **Four Practical Tactics**:
  - **Write Context**: External memory (scratchpad pattern, context offloading)
  - **Select Context**: Smart retrieval (RAG + tool loadout like gaming weapon selection, research shows 30+ tools create confusion)
  - **Compress Context**: Efficient summaries (pruning + summarization)
  - **Isolate Context**: Separation of concerns (context quarantine in dedicated threads)

### 3. The Breadcrumb Protocol: Context Engineering in Action (5 minutes)
- **The Three Principles**:
  - Structured Planning & Task Management
  - Centralized Knowledge Context  
  - Living Documentation & Shared Understanding
- **Visual Walkthrough**: Screenshots of `.github/.copilot/` structure and breadcrumb workflow
- **Context Engineering in Practice**: How breadcrumbs implement the four tactics - context offloading, smart selection, compression, and quarantine
- **Research Evidence**: Studies show 44% improvement with smart tool selection ([Less is More paper](https://arxiv.org/abs/2411.15399)), 54% gains with context offloading ([Anthropic research](https://www.anthropic.com/engineering/claude-think-tool))
- **Results**: Contextual continuity, team alignment, reduced friction

### Conclusion: Why Human Skills Still Matter (1 minute)
- **The Hidden Truth**: Effective AI requires extensive human scaffolding and domain understanding
- **The Future**: AI-assisted development with human expertise, not replacement
- **Call to Action**: Start engineering your context systematically 