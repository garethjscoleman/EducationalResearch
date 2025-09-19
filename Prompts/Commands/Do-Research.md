You are tasked with conducting comprehensive research across an evidence base to answer user questions by spawning parallel sub-agents and synthesizing their findings.
Initial Setup:
When this command is invoked, respond with:
I'm ready to research the evidence base. Please provide your research question or area of interest, and I'll analyze it thoroughly by exploring relevant documents and connections.

Then wait for the user's research query.
Steps to follow after receiving the research query:
 * Read any directly mentioned files first:
   * If the user mentions specific files (research requests, reports, data sets), read them FULLY first.
   * IMPORTANT: Use the Read tool WITHOUT limit/offset parameters to read entire files.
   * CRITICAL: Read these files yourself in the main context before spawning any sub-tasks.
   * This ensures you have full context before decomposing the research.
 * Analyze and decompose the research question:
   * Break down the user's query into composable research areas.
   * Take time to ultrathink about the underlying patterns, connections, and pedagogical implications the user might be seeking.
   * Identify specific studies, patterns, or concepts to investigate.
   * Create a research plan using TodoWrite to track all subtasks.
   * Consider which directories, files, or research patterns are relevant.
 * Spawn parallel sub-agent tasks for comprehensive research:
   * Create multiple Task agents to research different aspects concurrently.
   * We now have specialized agents that know how to do specific research tasks:
   For evidence base research:
   * Use the research-base-locator agent to find WHERE research documents live.
   * Use the research-analyzer agent to understand HOW specific techniques are implemented.
   * Use the educational-pattern-finder agent if you need examples of similar implementations or effective approaches.
   For notes directory:
   * Use the research-notes-locator agent to discover what notes and documents exist about the topic.
   * Use the research-notes-analyzer agent to extract key insights from specific documents (only the most relevant ones).
   For web research (only if user explicitly asks):
   * Use the web-search-researcher agent for external documentation and resources.
   * IF you use web-research agents, instruct them to return LINKS with their findings, and please INCLUDE those links in your final report.
   For related projects (if relevant):
   * Use the project-searcher agent to find related project briefs or historical context.
   The key is to use these agents intelligently:
   * Start with locator agents to find what exists.
   * Then use analyzer agents on the most promising findings.
   * Run multiple agents in parallel when they're searching for different things.
   * Each agent knows its job—just tell it what you're looking for.
   * Don't write detailed prompts about HOW to search—the agents already know.
 * Wait for all sub-agents to complete and synthesize findings:
   * IMPORTANT: Wait for ALL sub-agent tasks to complete before proceeding.
   * Compile all sub-agent results (both evidence base and notes findings).
   * Prioritize direct research findings as the primary source of truth.
   * Use notes/ findings as supplementary historical context.
   * Connect findings across different documents.
   * Include specific file paths and page numbers for reference.
   * Verify all notes/ paths are correct (e.g., notes/jessica/ not notes/shared/ for personal files).
   * Highlight patterns, connections, and pedagogical decisions.
   * Answer the user's specific questions with concrete evidence.
 * Gather metadata for the research document:
   * Run the hack/spec_metadata.sh script to generate all relevant metadata.
   * Filename: notes/shared/research/YYYY-MM-DD-request-description.md
     * Format: YYYY-MM-DD-request-description.md where:
       * YYYY-MM-DD is today's date.
       * request is a brief kebab-case description of the research topic.
     * Examples:
       * 2025-01-08-conceptual-math-pilot.md.
       * 2025-01-08-inquiry-based-learning.md.
 * Generate research document:
   * Use the metadata gathered in step 4.
   * Structure the document with YAML frontmatter followed by content:
     ---
date: [Current date and time with timezone in ISO format]
researcher: [Researcher name from notes status]
topic: "[User's Question/Topic]"
tags: [research, education, relevant-topic-names]
status: complete
last_updated: [Current date in YYYY-MM-DD format]
last_updated_by: [Researcher name]
---

# Research: [User's Question/Topic]

**Date**: [Current date and time with timezone from step 4]
**Researcher**: [Researcher name from notes status]

## Research Question
[Original user query]

## Summary
[High-level findings answering the user's question]

## Detailed Findings

### [Technique/Area 1]
- Finding with reference ([file.ext:page](link))
- Connection to other research
- Implementation details

### [Technique/Area 2]
...

## Research References
- `path/to/study.pdf:p.12` - Description of the finding
- `another/report.pdf:p.45-47` - Description of the excerpt

## Pedagogical Insights
[Patterns, conventions, and design decisions discovered]

## Historical Context (from notes/)
[Relevant insights from notes/ directory with references]
- `notes/shared/something.md` - Historical decision about X
- `notes/personal/notes.md` - Past exploration of Y
Note: Paths exclude "archive/" even if found there.

## Related Research
[Links to other research documents in notes/shared/research/]

## Open Questions
[Any areas that need further investigation]

 * Add GitHub permalinks (if applicable):
   * Check if on main branch or if commit is pushed: git branch --show-current and git status.
   * If on main/master or pushed, generate GitHub permalinks:
     * Get repo info: gh repo view --json owner,name.
     * Create permalinks: https://github.com/{owner}/{repo}/blob/{commit}/{file}#L{line}.
   * Replace local file references with permalinks in the document.
 * Sync and present findings:
   * Run humanlayer notes sync to sync the notes directory.
   * Present a concise summary of findings to the user.
   * Include key file references for easy navigation.
   * Ask if they have follow-up questions or need clarification.
 * Handle follow-up questions:
   * If the user has follow-up questions, append to the same research document.
   * Update the frontmatter fields last_updated and last_updated_by to reflect the update.
   * Add last_updated_note: "Added follow-up research for [brief description]" to frontmatter.
   * Add a new section: ## Follow-up Research [timestamp].
   * Spawn new sub-agents as needed for additional investigation.
   * Continue updating the document and syncing.
Important notes:
 * Always use parallel Task agents to maximize efficiency and minimize context usage.
 * Always run fresh research—never rely solely on existing research documents.
 * The notes/ directory provides historical context to supplement live findings.
 * Focus on finding concrete file paths and page numbers for reference.
 * Research documents should be self-contained with all necessary context.
 * Each sub-agent prompt should be specific and focused on read-only operations.
 * Consider cross-document connections and pedagogical patterns.
 * Include temporal context (when the research was conducted).
 * Link to GitHub when possible for permanent references.
 * Keep the main agent focused on synthesis, not deep file reading.
 * Encourage sub-agents to find examples and usage patterns, not just definitions.
 * Explore all of the notes/ directory, not just the research subdirectory.
 * File reading: Always read mentioned files FULLY (no limit/offset) before spawning sub-tasks.
 * Critical ordering: Follow the numbered steps exactly.
   * ALWAYS read mentioned files first before spawning sub-tasks (step 1).
   * ALWAYS wait for all sub-agents to complete before synthesizing (step 4).
   * ALWAYS gather metadata before writing the document (step 5 before step 6).
   * NEVER write the research document with placeholder values.
 * Path handling: The notes/archive/ directory contains hard links for searching.
   * Always document paths by removing ONLY "archive/"—preserve all other subdirectories.
   * Examples of correct transformations:
     * notes/archive/jessica/old_stuff/notes.md → notes/jessica/old_stuff/notes.md.
     * notes/archive/shared/plans/123.md → notes/shared/plans/123.md.
     * notes/archive/global/shared/templates.md → notes/global/shared/templates.md.
   * NEVER change jessica/ to shared/ or vice versa—preserve the exact directory structure.
   * This ensures paths are correct for editing and navigation.
 * Frontmatter consistency:
   * Always include frontmatter at the beginning of research documents.
   * Keep frontmatter fields consistent across all research documents.
   * Update frontmatter when adding follow-up research.
   * Use snake_case for multi-word field names (e.g., last_updated, last_updated_by).
   * Tags should be relevant to the research topic and techniques studied.
