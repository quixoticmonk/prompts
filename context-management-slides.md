# Managing AI Agent Context with Kiro CLI
## Preventing Context Rot and Maximizing Agent Effectiveness

---

## Slide 1: The Context Problem

**Context Rot**: AI agents become "dumber" as context grows
- Context window = sliding array of tokens
- No memory between sessions  
- Performance degrades with information overload
- Quality falls off rapidly around 100k tokens

*"They poison their own context... output quality falls off rapidly"* - Context Rot research

---

## Slide 2: Four Context Engineering Strategies

**From LangChain Research:**

1. **Write Context** - External memory & scratchpads
2. **Select Context** - Right information, right time
3. **Compress Context** - Summarize & trim intelligently
4. **Isolate Context** - Separate concerns & delegate

**Goal**: Give agents exactly what they need, when they need it

---

## Slide 3: The Information Overload Problem

**Common Anti-Patterns:**
- Dumping everything into system prompts
- 300+ line configuration files
- Task-specific details for every scenario
- Stale code snippets and outdated examples

**Result**: Agent ignores instructions due to irrelevance

---

## Slide 4: Progressive Disclosure Solution

**Better Approach:**
- **Universal Context**: Core project info only (~50 lines)
- **On-Demand Docs**: Task-specific knowledge when needed
- **Domain Specialists**: Focused agents for specific areas
- **Automated Loading**: Context appears based on task type

**Principle**: Load context progressively, not all at once

---

## Slide 5: Kiro's Smart Context Filtering

**Built-in Intelligence to Reduce Noise:**

✅ **Curated Documentation Sources**
```json
"toolsSettings": {
  "web_search": {
    "allowedDomains": ["docs.aws.amazon.com", "registry.terraform.io"],
    "maxResults": 5
  }
}
```

✅ **Content Processing & Limits**
```json
"resources": [
  {
    "type": "knowledgeBase", 
    "indexType": "best",
    "autoUpdate": true
  }
]
```

✅ **Progressive Loading**
```json
"resources": [
  "skill://.kiro/skills/**/SKILL.md"  // Metadata first, content on-demand
]
```

*"This aligns with Anthropic's own research on long-running agents, which emphasizes context window management, incremental progress, and memory systems that preserve learned patterns across sessions - exactly what Kiro CLI implements."*

**Result**: Agent gets precise, actionable information instead of information overload

---

## Slide 6: Context Compression in Action

**The Problem**: Long conversations hit context limits
- Important early context gets pushed out
- Agent "forgets" key decisions and context
- Performance degrades as window fills

**Kiro's Solution**: Automatic Compaction
- Summarizes conversation history intelligently
- Preserves key decisions and context
- Maintains continuity across long sessions
- Happens transparently in background

---

## Slide 7: Backpressure - Let Tools Do Tool Work

**Don't waste your time on trivial feedback**

❌ **Manual Correction**: "You forgot the import", "Wrong syntax"
✅ **Automated Feedback**: Agent runs tools and self-corrects

**Backpressure Sources:**
- Linters & formatters
- Type checkers & compilers
- Test suites & validation
- Build systems

**Result**: Agents work autonomously on complex tasks

---

## Slide 8: Kiro CLI's Context Management

**Built-in Context Engineering:**

1. **Skills System** - Auto-inject domain expertise (Select)
2. **Steering Docs** - Progressive knowledge disclosure (Select)
3. **Compaction** - Automatic conversation summarization (Compress)
4. **Subagent Delegation** - Isolated contexts for complex tasks (Isolate)
5. **Tool Integration** - Automated feedback loops (Write)

**Infrastructure for smart context management**

---

## Slide 9: Demo Setup - AWS Resource Import

**Scenario**: Import existing AWS resources into Terraform
- S3 buckets matching pattern: `company-*-data`
- IAM roles matching pattern: `*-service-role`

**Challenge**: Complex multi-step process requiring:
- AWS CLI knowledge
- Terraform import syntax
- Resource discovery patterns
- Configuration generation

---

## Slide 10: Demo - Before (No Skills)

**Manual Context Management:**
- User provides all AWS commands
- User explains Terraform import process
- User corrects syntax errors
- User guides resource discovery
- Context gets bloated with explanations

**Problems**: Slow, error-prone, context pollution

---

## Slide 11: Demo - After (With Skills)

**Automated Context Management:**
- Skills auto-inject AWS expertise
- Terraform knowledge loads on-demand
- Resource patterns recognized automatically
- Self-correction through tool feedback
- Clean, focused conversation

**Benefits**: Fast, accurate, maintainable context

---

## Slide 12: The Difference

**Without Skills:**
- 50+ messages of back-and-forth
- Manual error correction
- Context bloat from explanations
- User becomes the documentation
- **Context window fills up and "forgets" early decisions**

**With Skills + Compaction:**
- 5-10 focused messages
- Automated error handling
- Relevant context only
- Agent has built-in expertise
- **Automatic summarization preserves key context**

---

## Slide 13: Implementation Strategy

**Phase 1**: Identify repetitive tasks
- What do you explain repeatedly?
- Which domains need specialized knowledge?

**Phase 2**: Create focused skills
- Domain-specific expertise
- Progressive disclosure patterns
- Tool integration

**Phase 3**: Enable backpressure
- Automated validation
- Self-correction loops

---

## Slide 14: Key Takeaways

1. **Context is precious** - manage it like limited memory
2. **Progressive disclosure** beats information dumping  
3. **Compression preserves continuity** - automatic summarization prevents "forgetting"
4. **Backpressure enables autonomy** - tools provide feedback
5. **Skills prevent repetition** - encode expertise once
6. **Isolation prevents contamination** - separate concerns

**The Goal**: Agents that work smarter with the right context at the right time

---

## Demo Script

### Before Demo (No Skills)
```bash
kiro-cli chat
> I need to import existing S3 buckets and IAM roles into Terraform. 
> The buckets follow pattern company-*-data and roles are *-service-role

# Show manual process:
# - User explains AWS CLI commands
# - User provides Terraform import syntax  
# - User corrects errors
# - Context gets bloated
```

### After Demo (With Skills)
```bash
kiro-cli chat  
> Import existing AWS resources: S3 buckets matching company-*-data 
> and IAM roles matching *-service-role pattern

# Show automated process:
# - Skills auto-inject AWS knowledge
# - Terraform expertise loads automatically
# - Clean, focused execution
# - Self-correction through tools
```

## Key Talking Points

- **Context rot is measurable** - quality degrades with bloat
- **Progressive disclosure** solves the information timing problem
- **Skills encode expertise** - no need to repeat domain knowledge
- **Backpressure enables autonomy** - tools provide automated feedback
- **Kiro CLI provides infrastructure** - skills, steering, delegation
- **Demo shows dramatic difference** - efficiency and accuracy improvements
