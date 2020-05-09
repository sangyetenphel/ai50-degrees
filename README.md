# ai50-degrees

    $ python degrees.py large
    Loading data...
    Data loaded.
    Name: Emma Watson
    Name: Jennifer Lawrence
    3 degrees of separation.
    1: Emma Watson and Brendan Gleeson starred in Harry Potter and the Order of the Phoenix
    2: Brendan Gleeson and Michael Fassbender starred in Trespass Against Us
    3: Michael Fassbender and Jennifer Lawrence starred in X-Men: First Class

Video Demonstration: https://youtu.be/UeFLY48JEkw

In this project, we’re interested in finding the shortest path between any two actors by choosing a sequence of movies that connects them. For example, the shortest path between Jennifer Lawrence and Tom Hanks is 2: Jennifer Lawrence is connected to Kevin Bacon by both starring in “X-Men: First Class,” and Kevin Bacon is connected to Tom Hanks by both starring in “Apollo 13.”

We can frame this as a search problem: our states are people. Our actions are movies, which take us from one actor to another (it’s true that a movie could take us to multiple different actors, but that’s okay for this problem). Our initial state and goal state are defined by the two people we’re trying to connect. By using breadth-first search, we can find the shortest path from one actor to another.

While the implementation of search in lecture checks for a goal when a node is popped off the frontier, I improved the efficiency of the search by checking for a goal as nodes are added to the frontier: if we detect a goal node, no need to add it to the frontier, we can simply return the solution immediately.
