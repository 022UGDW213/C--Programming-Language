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

The line counts are measured, not estimated — `wc -l lesson-*/Program.cs` in the repo root:

```
   9 lesson-01-hello-world/Program.cs
  22 lesson-02-random-order-ids/Program.cs
  26 lesson-03-dice-roll/Program.cs
  57 total
```

## Run a lesson

Requires the [.NET 8 SDK](https://dotnet.microsoft.com/download) — all three projects target
`net8.0`. Run a lesson from inside its own folder:

```bash
cd lesson-01-hello-world
dotnet run
```

Use the same two commands in `lesson-02-random-order-ids` and `lesson-03-dice-roll`.

On the machine where these lessons were verified the SDK is installed in `/root/.dotnet` and is
not on `PATH` (`command -v dotnet` → not found), so `dotnet` there means `/root/.dotnet/dotnet`,
or `export PATH="/root/.dotnet:$PATH"` first. Running a compiled lesson straight from
`bin/Debug/net8.0/` additionally needs `DOTNET_ROOT=/root/.dotnet`.

## Verified

All three projects were built and run on **2026-09-27** with .NET SDK **8.0.425** and runtime
`Microsoft.NETCore.App` **8.0.31**:

```bash
/root/.dotnet/dotnet --version        # 8.0.425
/root/.dotnet/dotnet --list-runtimes  # Microsoft.NETCore.App 8.0.31 [/root/.dotnet/shared/...]
```

| Project | `dotnet build -v q --nologo` | compiled executable |
|---|---|---|
| `lesson-01-hello-world` | succeeded — 0 Warning(s), 0 Error(s) | exit 0; output identical on 5/5 runs |
| `lesson-02-random-order-ids` | succeeded — 0 Warning(s), 0 Error(s) | exit 0 on 200/200 runs |
| `lesson-03-dice-roll` | succeeded — 0 Warning(s), 0 Error(s) | exit 0 on 500/500 runs |

`lesson-01` is deterministic: the file holds two `Console.WriteLine` examples, so every run of
its executable printed exactly

```
Hello World!
Hello World!
```

Lessons 02 and 03 create an unseeded `Random`, so their output differs on every run. The figures
below are one measured batch together with the commands that produced it; re-running them gives
different random values of the same shape.

### lesson-02 — 200 runs, 1,000 order IDs

```bash
export DOTNET_ROOT=/root/.dotnet
BIN=lesson-02-random-order-ids/bin/Debug/net8.0/lesson-02-random-order-ids
for i in $(seq 1 200); do "$BIN" >> /tmp/lesson02.txt; done
```

* all 1,000 IDs matched `^[A-E][0-9]{3}$` — 0 malformed
* prefix distribution: A 212, B 190, C 212, D 203, E 183
* 897 of the 1,000 IDs were distinct
* the first five IDs of the batch were `B674`, `C727`, `A557`, `D446`, `A949`

### lesson-03 — 500 runs

```bash
export DOTNET_ROOT=/root/.dotnet
BIN=lesson-03-dice-roll/bin/Debug/net8.0/lesson-03-dice-roll
for i in $(seq 1 500); do "$BIN" >> /tmp/lesson03.txt; done
```

* every die landed in 1–6 — 0 of the 1,500 rolls were out of range
* the printed `Dice roll: a + b + c = total` always equalled `a + b + c` — 0 mismatches
* the bonus always matched the roll, `+2` on a pair and `+6` on triples — 0 mismatches
* 258 runs had no pair, 231 rolled doubles (`+2`), 11 rolled triples (`+6`)
* one run in full:

```
Dice roll: 1 + 1 + 6 = 8
You rolled doubles!  +2 bonus to total!
Total with bonus: 10
```

## Repository layout

```
lesson-01-hello-world/       Program.cs + lesson-01-hello-world.csproj
lesson-02-random-order-ids/  Program.cs + lesson-02-random-order-ids.csproj
lesson-03-dice-roll/         Program.cs + lesson-03-dice-roll.csproj
README.md
.gitignore                               bin/, obj/ and IDE state from `dotnet build`
.github/workflows/codeql-analysis.yml    CodeQL static analysis for the C# sources
```

Nine files are tracked (`git ls-files | wc -l` → 9); the `bin/` and `obj/` directories the
verification runs produced are ignored, not committed.
