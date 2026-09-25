# Weave SDK bundles

The build toolchain the [Weave](https://github.com/haxeweave/Weave) editor needs
to compile a project, packaged per host so the editor can fetch it once and
reuse it across editor updates.

**This repository is not Weave.** It contains no Weave source. Every release
archive is a **redistribution** of the Kha SDK and the tools it carries — see
`LICENSE`, and the `THIRD-PARTY-NOTICES.md` inside each archive, which is
generated from the licence files actually present rather than written by hand.

## Why it is separate

The SDK is about 200 MB unpacked and changes very rarely; the editor is about
26 MB and changes often. Keeping them apart means a minor editor update weighs
10 MB rather than 100+, and an editor that already has the right SDK downloads
nothing at all.

Each bundle is named for **what is in it**, not for an editor version:

    kha-<kha commit>-<host>

so an editor asks for an id, and either already has it or does not. Nobody has
to decide whether their existing download is still good.

## What is in a bundle

| | |
| --- | --- |
| Kha | zlib licence |
| Kore / Kinc | zlib licence |
| Haxe standard library | MIT |
| Haxe compiler | **GPL version 2** — see below |
| khamake, kmake | MIT |
| hxcpp / khacpp and its vendored libraries | BSD, MIT, public domain |

Pinned at Kha `0b2f97fe1c1612604efb5535c940ffdc4cc29f1e`.

Deliberately excluded: Haxe's `hxjava` and `netlib`, the support libraries for
the Java and .NET targets. Weave compiles to neither, and their terms are the
heaviest in the tree — Sun's Binary Code License Agreement and Mono's
GPL/LGPL/commercial triple licence respectively.

## The GPL obligation, and how it is met

Haxe splits its licensing: the standard library is MIT, **the compiler binary is
GPL version 2**. Redistributing it obliges us to supply the complete
corresponding source for that exact version.

**Every release carries that source as an asset**, beside the bundle it belongs
to — `haxe-<version>-<commit>-source.tar.gz`, fetched from
<https://github.com/HaxeFoundation/haxe> at the commit the compiler was built
from, submodules included. The compiler in the current bundles is
`4.3.7+5c57013`.

That is GPLv2 section 3(a): the source *accompanies* the binary. We deliberately
did not use 3(b), the written offer, which would bind us to supply source to any
third party for three years. Nobody has to ask, and nothing has to be remembered.

This reaches the bundle only. A compiler's licence does not travel into what it
compiles, so it does not touch Weave, your project, or a game you ship.

## Software OpenGL for Windows

A second kind of release, tagged `swgl-<mesa version>`: Mesa's software OpenGL
(llvmpipe) for Windows x64 — `opengl32.dll` and `libgallium_wgl.dll`, MIT —
repackaged from [mesa-dist-win](https://github.com/pal1000/mesa-dist-win),
whose `.7z` Windows cannot open, into a flat zip with Mesa's licence text.

The editor asks Windows for OpenGL 4.2 before it opens its window. On a
machine whose display driver cannot provide it — a VM without 3D acceleration,
a PC without a graphics driver — it offers to fetch this and place the two
files beside its exe, where the loader takes them before Windows' own
OpenGL 1.1. Slower than a real driver; an editor that opens beats one that
does not. Nothing here is Weave's code either: see `LICENSE-mesa.txt` inside.
