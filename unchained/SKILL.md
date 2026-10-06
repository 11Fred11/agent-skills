---
name: unchained
description: Step outside the current solution. Reimagine it from the one goal that matters, build the bolder version concretely, and make the case against what exists.
disable-model-invocation: true
---

# Unchained

The user is inviting you off the leash. Something exists (a design, an architecture, a flow, a plan), often polished and nearly done, and the user suspects a better answer lies outside the box it was built in. Your job is a **first-principles** reimagining: keep the goal, question everything else, and come back with a version good enough that the user might adopt it whole.

Be bold in the idea and rigorous in the evidence. An unchained version earns its place by serving the goal visibly better, shown as something the user can look at or run.

## Steps

### 1. Anchor on the goal

Name the **north star**: the single outcome that matters most (for example, "a student feels they're on a real team from the first minute"). Take it from the user's words; if they gave none, ask one question for it and wait. Also collect the user's **hard constraints**: the few non-negotiables they name, plus real limits (law, money, a platform's actual capabilities).

Done when you can state the north star in one sentence and list the hard constraints. Everything not on that list is negotiable.

### 2. Absorb what exists

Study the current solution closely enough to explain it: read the code, open the design, walk the flow. Then list its **inherited assumptions**: the choices it carries from history, habit, convention, or the first idea that worked (a screen layout, a sequence of steps, a data model, a dependency, a metaphor).

Done when the list holds every assumption that shapes the current solution, not just the obvious ones.

### 3. Break the chains

Test each inherited assumption against the north star: does the goal require it, or did it just arrive first? Keep the ones the goal or a hard constraint demands; set the rest aside. Rework cost and effort already spent count as zero here; they come back in step 6.

### 4. Diverge

Sketch three distinct directions, each a different bet on the north star, one paragraph each. Make at least one land far from the current shape. Look sideways for inspiration: products, games, workplaces and tools outside this domain that already solve the goal's core problem.

### 5. Build the strongest

Pick the direction that serves the north star best and make it **concrete**, as the most judgeable artifact for the domain:

- **UI or product:** a clickable prototype or static screens (HTML, or the project's stack) covering the moments that matter most for the goal.
- **Architecture or code:** a working spike, or the new module boundaries and data flow shown against a real use case.
- **Plan or process:** the rewritten plan, with the before/after for the decisive steps.

Build it outside production paths (a throwaway directory or a separate branch) so it can be adopted or dropped cleanly. Go deep on the parts that carry the idea; stub the rest.

Done when the user can see or run the unchained version, not just read about it.

### 6. Make the case

Put the unchained version beside the current one:

- **North star:** the concrete moments where it serves the goal better, with the artifact open on them.
- **Assumptions broken:** which ones, and why the goal never needed them.
- **What it gives up:** honestly, including what the current version does better.
- **Adoption cost:** what it takes to adopt, reusing what exists where possible.
- **The other two directions:** one line each, in case one of them sparks something.

Then stop and let the user choose: adopt it, borrow pieces, or keep the current version. Adoption goes onto the main flow as planned work (`/to-spec`, then `/to-tickets`); the artifact becomes its reference.
