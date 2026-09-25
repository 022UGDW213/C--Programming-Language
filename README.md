# C# Programming Language

Beginner C# console exercises — three small, standalone programs covering the basics of the
language. Each `lesson-*` folder is its own `net8.0` console project (top-level statements,
implicit usings, nullable enabled) and builds and runs on its own.

## Lessons

| Lesson | Project | What it covers | `Program.cs` lines |
|--------|---------|----------------|--------------------|
| 01 | `lesson-01-hello-world` | `Console.WriteLine`, string concatenation | 9 |
| 02 | `lesson-02-random-order-ids` | `Random`, arrays, `for`/`foreach` loops, string formatting | 22 |
| 03 | `lesson-03-dice-roll` | `Random`, conditionals, interpolated strings | 26 |

## Run a lesson

Requires the [.NET 8 SDK](https://dotnet.microsoft.com/download) — all three projects target
`net8.0`. Run a lesson from inside its own folder:

```bash
cd lesson-01-hello-world
dotnet run
```

Use the same two commands in `lesson-02-random-order-ids` and `lesson-03-dice-roll`.

## Verified

Built and run on **2026-09-26** with .NET SDK **8.0.425** (runtime `Microsoft.NETCore.App`
`8.0.31`):

| Project | `dotnet build` | `dotnet run` |
|---|---|---|
| `lesson-01-hello-world` | succeeded — 0 warnings, 0 errors | exit 0 |
| `lesson-02-random-order-ids` | succeeded — 0 warnings, 0 errors | exit 0 |
| `lesson-03-dice-roll` | succeeded — 0 warnings, 0 errors | exit 0 |

`lesson-01` prints `Hello World!` twice — once from each of the two examples in the file — so
its output is fixed. Lessons 02 and 03 create an unseeded `Random`, so their output is different
on every run; one real run of lesson 02 was:

```
A961
D705
B134
D625
D193
```

Behaviour was checked by running the compiled executables repeatedly: 200 runs of `lesson-02`
(1,000 order IDs) produced only IDs matching `[A-E][0-9]{3}`, and across 500 runs of
`lesson-03` every die roll landed in 1–6, the printed total always equalled the sum of the three
rolls, and the bonus was applied correctly — `+2` for doubles on 216 runs and `+6` for triples
on 15 runs.

## Repository layout

```
lesson-01-hello-world/       Program.cs + lesson-01-hello-world.csproj
lesson-02-random-order-ids/  Program.cs + lesson-02-random-order-ids.csproj
lesson-03-dice-roll/         Program.cs + lesson-03-dice-roll.csproj
README.md
.github/workflows/codeql-analysis.yml   CodeQL static analysis for the C# sources
```
