# Art analysis of TheBrain

This has been done in 2025

The main objective is to import a publication map into *TheBrain*.

## What is TheBrain?

It is a sofware released in 1998 from [*TheBrain Technologies*](en.wikipedia.org/wiki/TheBrain_Technologies).

The sofware is specialized in a *Knowledge Graph* type of *mind mapping* software.
Go and see that video to understand: [*video*](https://assets.thebrain.com/app/GettingStartedPhone-01.mp4).

The power of *TheBrain* is that it can process a **large amount of data**,
more than any of its competitors.

## Exportation

The data from *TheBrain* can be exported in *json*.
To understand the different files, you can check the explanations
from the former internship (2023).

In that [*forum*](https://forums.thebrain.com/post/json-export-anyone-try-or-have-success-12553459),
some people discussed about the difficulty they faced to manipulate *json* data.
In fact, *TheBrain* removed the *import/export* via *CSV*,.

They said: ``The export features of this product are HORRIBLE``, ``ZERO help``.

## Importation

Import via *json* is not supported as it is too easy to create invalid data.
There are two other ideas:

- import via *XML*: requires a parsing from *json* to *xml*.

It could depend on the *TheBrain* version used.

- import via the **API**.

The version *1.0.5* has been released in 2023. It seems to be great.

Here is the [*swagger*](https://api.bra.in).

They even made some example projects [*right here*](https://github.com/TheBrainTech/thebrain-api-quickstart-node).

**I need to investigate about it**.

## Competitors

Hard to say. *TheBrain* seems to be the best.

## Database that could be used to manipulate *TheBrain* data.

Of course, *TheBrain* has its own database.
However, we can't use it except with the *API*.

*neo4j*(Graph database) could be used to manipulate data and then,
it can be send to *TheBrain* via the *API*.

**Neo4j** seems to be more *dev friendly* even if it is not fully *open-source*.
It is a *NO-SQL* databased that's *Java* based.

Here is the [*github*](https://github.com/neo4j).
I already found a [*docker image right here*](https://hub.docker.com/_/neo4j).

Other alternatives of *graph databases* could be is explained [*here*](https://www.graphable.ai/software/what-is-neo4j-graph-database).

### EOF

