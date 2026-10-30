# Degrees of Separation

A command-line program that finds the shortest chain of movies connecting any two actors, in the spirit of "Six Degrees of Kevin Bacon". It treats actors as states and shared movies as the links between them, and uses breadth-first search to find the shortest path. This is the Degrees project from Harvard's CS50 Introduction to Artificial Intelligence with Python.

![Running degrees.py: Emma Watson to Jennifer Lawrence in 3 steps with the large dataset, then Tom Cruise to Cary Elwes in 4 steps with the small dataset](docs/demo.gif)

The demo shows real output from the program. The wait while the large dataset loads has been shortened.

Video walkthrough: https://youtu.be/UeFLY48JEkw

## What it does

- Loads a dataset of people, movies and who starred in what from CSV files.
- Asks for two names and prints how many degrees of separation there are, followed by each movie in the chain, for example "Emma Watson and Ezra Miller starred in The Perks of Being a Wallflower".
- If a name matches more than one person, it lists each match with their birth year and asks for the intended person's ID.
- Prints "Person not found." if a name isn't in the dataset.

## How it works

### The data

`load_data` reads three CSV files into dictionaries:

- `people`: person ID → name, birth year and the set of movies they starred in
- `movies`: movie ID → title, year and the set of people who starred in it
- `names`: lowercase name → the set of person IDs with that name (used for lookups and to detect duplicate names)

`neighbors_for_person(person_id)` returns every `(movie_id, person_id)` pair for people who share a movie with the given person.

### Search problem

- **State:** a person
- **Action:** a movie, which moves from one person to a co-star
- **Initial state:** the first person entered
- **Goal:** the second person entered

### Breadth-first search

`shortest_path(source, target)` runs breadth-first search (BFS):

1. A `QueueFrontier` (first in, first out) starts with a node for the source person. Each node stores a person, the node it came from (its parent) and the movie that connects them.
2. The loop removes the oldest node from the frontier. If it is the target, the path is rebuilt by following parent links back to the source.
3. Otherwise, each co-star who is not already in the frontier or in the explored set is added to the frontier as a new node.
4. If the frontier becomes empty, the function returns `None`.

Because BFS explores everyone one movie away before anyone two movies away, the first path that reaches the target is a shortest one.

One change from the version shown in lecture: the code also checks for the goal when a node is added to the frontier, not only when it is removed. If a co-star is the target, the path is returned straight away, which saves expanding the rest of that level of the search.

Several different paths can have the same shortest length. The program returns one of them, and which one can change between runs, because co-stars are stored in a Python set.

## What I implemented

The course provides `load_data`, `main`, `person_id_for_name`, `neighbors_for_person` and the `Node`, `StackFrontier` and `QueueFrontier` classes in `util.py`. I wrote `shortest_path`, the breadth-first search.

## Known limitation

The explored set stores search nodes, but the code checks it for person IDs, so a person who has already been explored can be added to the frontier again. When two people are connected, BFS still finds a shortest path. When they are not connected and the first person has co-stars, the search does not end, so the "Not connected." message is only reached when the first person has no co-stars.

## Project structure

```
degrees.py   Loads the data, reads the two names and runs the search (shortest_path)
util.py      Node, StackFrontier and QueueFrontier classes
small/       A 16-person, 5-movie dataset for quick testing
large/       The full dataset: about 1,044,000 people, 344,000 movies and 1,190,000 star credits
```

Each dataset folder has `people.csv`, `movies.csv` and `stars.csv`.

## Running it locally

The program uses only the Python standard library, so there is nothing to install. It was tested with Python 3.13.

```bash
git clone https://github.com/sangyetenphel/ai50-degrees.git
cd ai50-degrees
python3 degrees.py small     # quick test with the small dataset
python3 degrees.py large     # full dataset (the default if no folder is given)
```

Enter the two names when asked, for example `Kevin Bacon` and `Tom Hanks` for the small dataset. The large dataset takes around half a minute to load and search.

## Built with

- Python 3 (standard library only)

## Credits

This is a project from [CS50's Introduction to Artificial Intelligence with Python](https://cs50.harvard.edu/ai/) by Harvard University. The project specification, starter code and the movie datasets come from the course.

Created by Sangye Tenphel, 2020.
