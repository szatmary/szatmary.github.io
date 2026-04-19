---
title: "Four Months of Saturdays"
date: 2026-04-18
tags: ["cad", "3d-printing", "facet"]
summary: "I built a CAD program because the ones I had didn't fit what I wanted to do."
---

These days, the CAD program I use most is [one I wrote myself](https://github.com/firstlayer-xyz/Facet). Two years ago, a program like this would have taken a team. What happened in between is that the tools I design with stopped fitting me, and the tools I code with started doing a lot more of the work.

![Facet with a Golden Gate Bridge model in the viewport and an AI assistant panel discussing how to color the deck.](/images/facet-aiview.png)

I invested in my first 3D printer during the COVID lockdown. For a nerd like me it was close to an ideal hobby: part engineering, part craft, part excuse to tinker, with a direct loop from "thing in my head" to "thing in my hand." As a programmer, I was used to the first half of that loop (take an idea, turn it into something usable), but the output was always pixels on a screen. This time it was a physical object I could set on my desk. The hobby has changed a lot since then. What used to be a finicky kit you tuned for hours before getting a decent print is now a box that mostly just works, at a price point that would have been unbelievable five years ago. The hardware is genuinely good now, and it's getting better and cheaper fast.

What I actually print hasn't changed much. Brackets, enclosures, spacers, adapters, replacement parts for things that broke, and the occasional print just for fun. Nothing that needs professional CAD software. Most CAD programs are not built for me. They're built for mechanical engineers at real companies, designing real assemblies that will eventually be machined or injection molded. They have ribbon menus full of features I will never use, they're expensive, and they have a learning curve measured in weeks or months. And rightfully so. These are serious, complicated programs, built by skilled programmers to solve real engineering problems. Every time I've tried to sit down with one of them to design a bracket, though, I've spent an hour watching tutorials and left without a bracket. I don't want to learn a new vocabulary of workflows to punch four holes in a flat plate.

## OpenSCAD was closer

OpenSCAD felt much more like home. As a programmer, describing geometry as code is how I already think about it. You write a little script that says "take this cube, subtract this cylinder, translate the result," and a 3D model falls out. For simple parts this is wonderful. A lot of what I've printed over the years started life as an OpenSCAD file.

But the longer I used it, the more small frustrations added up.

Official releases have essentially stopped. There are nightly builds, and that's what most people end up running, but a project without a real release cadence starts to feel abandoned even when people are still working on it. Composability is awkward. Passing shapes between modules, parameterizing designs cleanly, and building anything resembling reusable abstractions all feel like fighting the language rather than using it. And the library ecosystem is thin. There's no real standard library, and no package manager worth the name.

The tipping point for me was a threaded hole. I wanted a screw hole with real threads, so I went looking for a library and picked the one most people seemed to recommend. I imported it, called the module with what looked like reasonable parameters, and hit render. OpenSCAD pinned a CPU core at 100% and sat there. I waited. I waited some more. Eventually I force-quit and lost the morning's work, because of course I hadn't saved.

When I went back and read the library source, the problem turned out to be an infinite loop triggered by leaving one of the optional parameters undefined. Not a crash, not an error message. Just a silent spin forever.

I continued to use OpenSCAD, and I've always been able to get the part I needed out of it one way or another.

## Four Months of Saturdays

Over the last year or two, AI has quietly raised the ceiling on what one or two people can build in their spare time, to the point where "write your own CAD program" stopped being a ridiculous answer to "the tools I have don't fit."

Late last year I was working on a side project that used [Manifold](https://github.com/elalish/manifold), a fast and robust mesh boolean library. It was already sitting in my editor, already compiled, already doing the hard part of 3D CAD for me. I had a geometry kernel on one monitor and a problem that needed geometry on the other. The shortest line between them was to stop trying to wrangle OpenSCAD and write just enough of my own tool.

I didn't intend to build a full CAD program, just something for my personal needs. But once you have primitives, transforms, and boolean operations working, it is difficult not to keep going, because every feature you add makes the next one obvious.

Live debugging went in. Physical units became first-class types, so `8 mm` and `45 deg` are real values the compiler understands, not floats you hope are in the right scale. The language grew into something that felt like a real language instead of a macro system. I added the ability to click a face in the 3D viewport and jump straight to the line of code that created it. Parameters with ranges turned into sliders in the UI, so I could drag a value and watch the model respond. Library imports got a package manager shaped like Go's, so you can pull a library straight from a Git URL.

Four months of Saturdays later: [Facet](https://github.com/firstlayer-xyz/Facet). I finally have a CAD package I actually like using. It's probably not for everyone, it's still a little buggy, and frankly I don't know what its future looks like.

![A parametric rocket in Facet, with sliders driving radius, height, and fin count.](/images/facet-rocket.png)

And because I can't resist scratching my own itches: the library I wanted for threaded holes, years after that OpenSCAD morning, is a few lines of import away.

## Gourmet slop

The part of this project I keep coming back to is that two years ago I would not have attempted it. Not because the ideas are especially hard, but because the surface area is massive. A parser. A type system. A renderer. A viewport with picking. Slicer integration. A CLI. Documentation. That is a year of work for a small team, not a weekend project for one person.

What changed is that a significant chunk of writing the code for this is something I supervise rather than something I do. I still make the design decisions, the architecture, the taste calls about what the language should feel like, and I put a lot of personal time and experience into it. But a lot of the mechanical work in between (the boilerplate, the wiring, the "please implement this straightforward thing across these four files") is something I can hand off and review. That shifts the ceiling on what one person can hold in their head and ship.

Claude wasn't always the best collaborator. The domain-specific language required excruciating micromanagement. I lost count of how many times I told it "I don't care that it's a big change." And more than once I had to warn it that if it implemented one more `fallback` instead of just figuring out how to handle the situation, I'd drown it in a lake.

But when it was on task, the gains were real. A cross-file refactor that would have eaten days shrinks to hours of nudging and redirecting. A feature that would have stalled on "I don't fell like writing that rite now" gets done before I lose interest.

Facet couldn't exist without that shift. It's not that AI wrote it (although it did); it's that AI changed the cost of building it enough that it stopped being ridiculous to try. These days Facet is my primary design tool for the modest amount of CAD work I do, and I keep adding features as I need or want them.

Facet is free and open source. If you're the kind of person who'd rather describe a part than click through a menu, you might get some use out of it.
