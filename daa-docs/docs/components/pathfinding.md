# Pathfinding Algorithms Visualizer

Welcome to the **Pathfinding Algorithms Visualizer** module, a core component of the DAA for Kids project. This interactive tool is designed to illuminate the fascinating world of algorithms that find the shortest or optimal path between two points in a graph or grid. Understanding pathfinding is fundamental to many areas of computer science, from game development and robotics to network routing and logistics.

---

## What is Pathfinding?

At its heart, pathfinding is about navigating from a starting point to a destination, often avoiding obstacles and sometimes considering the "cost" of traversing different paths. Our visualizer brings these abstract concepts to life on a grid, allowing you to see exactly how various algorithms explore possible routes and determine the most efficient path.

---

## Using the Visualizer

The visualizer provides a dynamic grid environment where you can:

* **Define the Environment:** Place start and end nodes, draw walls to represent impassable obstacles, and add weights to cells to simulate varying traversal costs.

* **Select an Algorithm:** Choose from a diverse set of pathfinding algorithms.

* **Control Execution:** Start, pause, resume, and adjust the speed of the visualization to observe the algorithm's step-by-step process.

* **Reset and Generate:** Clear the grid or generate random maps to test algorithms under different conditions.

As the algorithm runs, you'll see nodes being visited, the path being constructed (if found), and understand the search pattern unique to each algorithm.

---

## Algorithms Explored

This visualizer offers a comprehensive look at a variety of pathfinding algorithms, categorized by their approach and suitability for different types of graphs (weighted vs. unweighted). Each algorithm brings a unique strategy to the problem of finding a path.

### Unweighted Graph Algorithms

These algorithms are typically used on graphs where the cost of moving between any two connected nodes is the same (or assumed to be 1). They focus on finding the path with the fewest steps.

* **Breadth-First Search (BFS):** Explores the graph level by level, guaranteeing the shortest path in terms of the number of edges.

* **Depth-First Search (DFS):** Explores as far down one branch as possible before backtracking, often used for traversal but not guaranteed to find the shortest path.

### Weighted Graph Algorithms

When edges or nodes have different associated costs (weights), these algorithms are necessary to find the path with the minimum total cost.

* **Dijkstra’s Algorithm:** Finds the shortest paths from a single source node to all other nodes in a graph with non-negative edge weights.

* **A\* Search Algorithm:** An extension of Dijkstra's that uses a heuristic function to guide its search, making it more efficient in many cases, especially for finding a path to a specific destination.

* **Greedy Best-First Search:** Uses a heuristic to prioritize exploration towards the target, but does not guarantee the shortest path.

* **Bellman-Ford Algorithm:** Can find the shortest paths from a single source node even in graphs with negative edge weights (though it detects negative cycles).

* **Floyd-Warshall Algorithm:** Finds the shortest paths between *all* pairs of nodes in a weighted graph.

### Advanced and Specialized Algorithms

This category includes algorithms designed for specific scenarios or offering alternative approaches to pathfinding.

* **Bidirectional Search:** Runs two searches simultaneously, one from the start and one from the end, meeting in the middle. Can be faster than a single-direction search.

* **Jump Point Search:** An optimization for grid-based pathfinding that "jumps" over large areas, significantly speeding up search on uniform cost grids.



---

## Algorithm Pseudocode

Explore the fundamental logic behind each pathfinding algorithm. Select an algorithm from the dropdown below to view its pseudocode.

<div class="algorithm-pseudocode-container">
    <select id="algorithm-selector" class="algorithm-dropdown">
        <option value="">Select an Algorithm</option>
        <option value="bfs">Breadth-First Search (BFS)</option>
        <option value="dijkstra">Dijkstra’s Algorithm</option>
        <option value="astar">A* Search Algorithm</option>
        <option value="greedy-bfs">Greedy Best-First Search</option>
        <option value="bellman-ford">Bellman-Ford Algorithm</option>
        <option value="floyd-warshall">Floyd-Warshall Algorithm</option> {/* <-- Check this value */}
        <option value="bidirectional">Bidirectional Search</option>
        <option value="jps">Jump Point Search</option>
        </select>

    <div id="pseudocode-display" class="pseudocode-block">
        <p>Select an algorithm from the dropdown above to see its pseudocode.</p>
    </div>

    <div id="bfs-pseudocode" class="pseudocode-content" style="display: none;">
        ```python
        BFS(graph, start_node, end_node):
            create a queue Q
            add start_node to Q
            mark start_node as visited
            create a map parent to store parent of each node

            while Q is not empty:
                current_node = Q.dequeue()

                if current_node is end_node:
                    return reconstruct_path(parent, start_node, end_node)

                for each neighbor of current_node:
                    if neighbor is not visited:
                        mark neighbor as visited
                        set parent[neighbor] = current_node
                        Q.enqueue(neighbor)

            return "No path found"

        reconstruct_path(parent, start_node, end_node):
            path = []
            current = end_node
            while current is not null:
                path.add(current)
                current = parent[current]
            reverse path
            return path
        ```
    </div>

    <div id="dijkstra-pseudocode" class="pseudocode-content" style="display: none;">
        ```python
        Dijkstra(graph, start_node, end_node):
            create a priority queue PQ
            create a map distance, initialize all to infinity, distance[start_node] = 0
            create a map parent to store parent of each node

            PQ.add(start_node, 0) # node, priority

            while PQ is not empty:
                current_node = PQ.extract_min()

                if current_node is end_node:
                    return reconstruct_path(parent, start_node, end_node)

                for each neighbor of current_node:
                    edge_weight = weight(current_node, neighbor)
                    new_distance = distance[current_node] + edge_weight

                    if new_distance < distance[neighbor]:
                        distance[neighbor] = new_distance
                        parent[neighbor] = current_node
                        if neighbor is in PQ:
                            PQ.decrease_priority(neighbor, new_distance)
                        else:
                            PQ.add(neighbor, new_distance)

            return "No path found"

        reconstruct_path(parent, start_node, end_node):
            path = []
            current = end_node
            while current is not null:
                path.add(current)
                current = parent[current]
            reverse path
            return path
        ```
    </div>

    <div id="astar-pseudocode" class="pseudocode-content" style="display: none;">
        ```python
        A_star(graph, start_node, end_node):
            create a priority queue open_set
            create a map came_from to store parent of each node
            create a map g_score, initialize all to infinity, g_score[start_node] = 0
            create a map f_score, initialize all to infinity, f_score[start_node] = heuristic(start_node, end_node)

            open_set.add(start_node, f_score[start_node]) # node, priority

            while open_set is not empty:
                current_node = open_set.extract_min()

                if current_node is end_node:
                    return reconstruct_path(came_from, start_node, end_node)

                for each neighbor of current_node:
                    tentative_g_score = g_score[current_node] + distance(current_node, neighbor)

                    if tentative_g_score < g_score[neighbor]:
                        came_from[neighbor] = current_node
                        g_score[neighbor] = tentative_g_score
                        f_score[neighbor] = g_score[neighbor] + heuristic(neighbor, end_node)
                        if neighbor not in open_set:
                            open_set.add(neighbor, f_score[neighbor])

            return "No path found"

        heuristic(node_a, node_b):
            // Implement a suitable heuristic function (e.g., Manhattan distance, Euclidean distance)
            return estimated_distance_between(node_a, node_b)

        reconstruct_path(came_from, start_node, end_node):
            path = []
            current = end_node
            while current is not null:
                path.add(current)
                current = came_from[current]
            reverse path
            return path
        ```
    </div>

    <div id="greedy-bfs-pseudocode" class="pseudocode-content" style="display: none;">
        ```python
        Greedy_Best_First_Search(graph, start_node, end_node):
            create a priority queue PQ
            create a map came_from to store parent of each node
            create a set visited

            PQ.add(start_node, heuristic(start_node, end_node)) # node, priority based on heuristic

            while PQ is not empty:
                current_node = PQ.extract_min()

                if current_node is end_node:
                    return reconstruct_path(came_from, start_node, end_node)

                if current_node is in visited:
                    continue
                mark current_node as visited

                for each neighbor of current_node:
                    if neighbor not in visited:
                        came_from[neighbor] = current_node
                        PQ.add(neighbor, heuristic(neighbor, end_node))


            return "No path found"

        heuristic(node_a, node_b):
            // Implement a suitable heuristic function (e.g., Manhattan distance, Euclidean distance)
            return estimated_distance_between(node_a, node_b)

        reconstruct_path(came_from, start_node, end_node):
            path = []
            current = end_node
            while current is not null:
                path.add(current)
                current = came_from[current]
            reverse path
            return path
        ```
    </div>

    <div id="bellman-ford-pseudocode" class="pseudocode-content" style="display: none;">
        ```python
        Bellman_Ford(graph, start_node):
            create a map distance, initialize all to infinity, distance[start_node] = 0
            create a map parent to store parent of each node

            num_vertices = number of vertices in graph

            // Relax edges |V| - 1 times
            repeat num_vertices - 1 times:
                for each edge (u, v) with weight w in graph:
                    if distance[u] + w < distance[v]:
                        distance[v] = distance[u] + w
                        parent[v] = u

            // Check for negative cycles
            for each edge (u, v) with weight w in graph:
                if distance[u] + w < distance[v]:
                    return "Graph contains a negative cycle" // Cannot find shortest paths

            return distance, parent // Returns shortest distances from start_node and parent pointers

        // Note: To find the path to a specific end_node, you would reconstruct it
        // using the parent map after the algorithm completes and confirms no negative cycles.
        reconstruct_path(parent, start_node, end_node):
            path = []
            current = end_node
            // Add a check to prevent infinite loops in case of issues or unreachable nodes
            visited_check = set()
            while current is not null and current not in visited_check:
                path.add(current)
                visited_check.add(current)
                current = parent[current]
            // Check if start_node was reached (current is null if not)
            if current is null and start_node not in path:
                 return "Path could not be reconstructed" # Or handle as an error
            reverse path
            return path
        ```
    </div>

    <div id="floyd-warshall-pseudocode" class="pseudocode-content" style="display: none;"> {/* <-- Check this ID */}
        ```python
        Floyd_Warshall(graph):
            num_vertices = number of vertices in graph
            create a matrix dist of size num_vertices x num_vertices
            create a matrix next_node of size num_vertices x num_vertices // To reconstruct paths

            // Initialize dist matrix
            for i from 0 to num_vertices - 1:
                for j from 0 to num_vertices - 1:
                    if i == j:
                        dist[i][j] = 0
                    elif edge exists from i to j:
                        dist[i][j] = weight(i, j)
                        next_node[i][j] = j
                    else:
                        dist[i][j] = infinity
                        next_node[i][j] = null

            // Apply the algorithm
            for k from 0 to num_vertices - 1: // Intermediate vertices
                for i from 0 to num_vertices - 1: // Start vertices
                    for j from 0 to num_vertices - 1: // End vertices
                        if dist[i][k] is not infinity and dist[k][j] is not infinity:
                            if dist[i][k] + dist[k][j] < dist[i][j]:
                                dist[i][j] = dist[i][k] + dist[k][j]
                                next_node[i][j] = next_node[i][k]

        // dist matrix now contains shortest paths between all pairs
        // next_node matrix can be used to reconstruct paths

        return dist, next_node

    reconstruct_path_floyd_warshall(start_node, end_node, next_node):
         if next_node[start_node][end_node] is null:
             return "No path exists" // Or handle as error

         path = [start_node]
         current = start_node
         while current is not end_node:
             current = next_node[current][end_node]
             path.add(current)

         return path
        ```
    </div>

    <div id="bidirectional-pseudocode" class="pseudocode-content" style="display: none;">
        ```python
        Bidirectional_Search(graph, start_node, end_node):
            create queue_forward and queue_backward
            create visited_forward and visited_backward sets
            create parent_forward and parent_backward maps

            queue_forward.enqueue(start_node)
            visited_forward.add(start_node)
            parent_forward[start_node] = null

            queue_backward.enqueue(end_node)
            visited_backward.add(end_node)
            parent_backward[end_node] = null

            while queue_forward is not empty and queue_backward is not empty:
                // Search from start
                current_f = queue_forward.dequeue()
                if current_f in visited_backward:
                    return reconstruct_bidirectional_path(parent_forward, parent_backward, current_f)

                for each neighbor of current_f:
                    if neighbor not in visited_forward:
                        visited_forward.add(neighbor)
                        parent_forward[neighbor] = current_f
                        queue_forward.enqueue(neighbor)

                // Search from end
                current_b = queue_backward.dequeue()
                if current_b in visited_forward:
                    return reconstruct_bidirectional_path(parent_forward, parent_backward, current_b)

                for each neighbor of current_b:
                    if neighbor not in visited_backward:
                        visited_backward.add(neighbor)
                        parent_backward[neighbor] = current_b
                        queue_backward.enqueue(neighbor)

            return "No path found" // Queues are empty and no intersection found

        reconstruct_bidirectional_path(parent_forward, parent_backward, meeting_node):
            path = []
            // Path from start to meeting_node
            current = meeting_node
            while current is not null:
                path.add(current)
                current = parent_forward[current]
            reverse path

            // Path from meeting_node to end_node
            current = parent_backward[meeting_node] // Start from the node *before* the meeting node in backward search
            while current is not null:
                path.add(current)
                current = parent_backward[current]

            return path
        ```
    </div>

    <div id="jps-pseudocode" class="pseudocode-content" style="display: none;">
        ```python
        // Note: Jump Point Search is complex and typically implemented on a grid.
        // This pseudocode provides a high-level overview of the core idea.

        JPS(grid, start_node, end_node):
            create open_set (priority queue)
            create came_from map
            create g_score map, initialize all to infinity, g_score[start_node] = 0
            create f_score map, initialize all to infinity, f_score[start_node] = heuristic(start_node, end_node)

            open_set.add(start_node, f_score[start_node])

            while open_set is not empty:
                current_node = open_set.extract_min()

                if current_node is end_node:
                    return reconstruct_path(came_from, start_node, end_node)

                // Find "jump points" from the current node
                neighbors = find_jump_points(current_node, came_from[current_node], grid)

                for each neighbor (jump_point):
                    tentative_g_score = g_score[current_node] + distance(current_node, jump_point)

                    if tentative_g_score < g_score[jump_point]:
                        came_from[jump_point] = current_node
                        g_score[jump_point] = tentative_g_score
                        f_score[jump_point] = g_score[jump_point] + heuristic(jump_point, end_node)
                        if jump_point not in open_set:
                            open_set.add(jump_point, f_score[jump_point])

            return "No path found"

        find_jump_points(current_node, parent_node, grid):
            // This is the core complex part of JPS.
            // It involves pruning redundant neighbors and recursively searching for forced neighbors
            // and jump points in cardinal and diagonal directions.
            // Returns a list of significant "jump points" reachable from current_node.
            pass // Placeholder for complex logic

        heuristic(node_a, node_b):
            // Implement a suitable heuristic function (e.g., Manhattan distance, Euclidean distance)
            return estimated_distance_between(node_a, node_b)

        reconstruct_path(came_from, start_node, end_node):
            path = []
            current = end_node
            while current is not null:
                path.add(current)
                current = came_from[current]
            reverse path
            return path
        ```
    </div>

    </div>

<script>
document.addEventListener('DOMContentLoaded', function() {
    const selector = document.getElementById('algorithm-selector');
    const pseudocodeDisplay = document.getElementById('pseudocode-display');
    const pseudocodeContentBlocks = document.querySelectorAll('.pseudocode-content');

    console.log("Script loaded. Pseudocode content blocks found:", pseudocodeContentBlocks.length);

    selector.addEventListener('change', function() {
        const selectedAlgorithm = selector.value;
        console.log('Selected algorithm:', selectedAlgorithm);

        // Hide all pseudocode blocks
        pseudocodeContentBlocks.forEach(block => {
            block.style.display = 'none';
        });

        // Clear the main display area
        pseudocodeDisplay.innerHTML = '';

        if (selectedAlgorithm) {
            const targetBlockId = selectedAlgorithm + '-pseudocode';
            console.log('Attempting to find target block with ID:', targetBlockId); // More specific log
            const targetBlock = document.getElementById(targetBlockId);

            if (targetBlock) {
                console.log('Target block FOUND:', targetBlock); // Log if found
                // Find the pre element directly in the hidden target block
                const preElement = targetBlock.querySelector('pre');
                console.log('Found pre element:', preElement);

                if (preElement) {
                     // Clone the pre element and append it to the display area
                    const clonedPreElement = preElement.cloneNode(true);
                    pseudocodeDisplay.appendChild(clonedPreElement);
                    console.log('Pseudocode appended and should be visible.');

                    // Re-highlight the code block if necessary.
                    // MkDocs Material often handles this automatically for elements
                    // added to the DOM, but if not, you might need to trigger it.
                    // The exact method depends on the highlighting library used by MkDocs Material.
                    // A common approach if using highlight.js would be:
                    // if (window.hljs) {
                    //     window.hljs.highlightBlock(clonedPreElement.querySelector('code'));
                    // }
                    // If using Prism.js:
                    // if (window.Prism) {
                    //     window.Prism.highlightElement(clonedPreElement.querySelector('code'));
                    // }
                    // Test this on your site to see if automatic re-highlighting works.
                } else {
                     console.error('Could not find pre element within target block:', targetBlock);
                     pseudocodeDisplay.innerHTML = '<p>Error loading pseudocode.</p>';
                }

            } else {
                console.error('Could not find target block with ID:', targetBlockId); // Log if NOT found
                pseudocodeDisplay.innerHTML = '<p>Error loading pseudocode.</p>';
            }
        } else {
             pseudocodeDisplay.innerHTML = '<p>Select an algorithm from the dropdown above to see its pseudocode.</p>';
        }
    });

    // Optional: You might need to trigger highlighting on initial page load
    // for all code blocks if they are not highlighted by default when hidden.
    // This is often handled by MkDocs Material itself.
});
</script>

<style>
/* Improved styling for the dropdown and container */
.algorithm-pseudocode-container {
    margin: 20px 0; /* Add some vertical margin */
    padding: 20px; /* Increased padding */
    background-color: #282c34; /* A common dark theme background color (like VS Code's default) */
    border-radius: 8px; /* Slightly more rounded corners */
    border: 1px solid #3a3f4b; /* Subtle border color */
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3); /* Add a subtle shadow */
    font-family: 'Roboto', sans-serif; /* Use a clean font, maybe match your theme's font */
}

.algorithm-dropdown {
    display: block; /* Make it a block element for better layout */
    width: 100%; /* Make it fill the container width */
    padding: 12px 15px; /* Increased padding */
    border-radius: 5px; /* Rounded corners for the dropdown */
    border: 1px solid #555; /* Border color */
    background-color: #3b4048; /* Darker background for the dropdown */
    color: #abb2bf; /* Text color (common in dark themes) */
    font-size: 1rem;
    cursor: pointer;
    margin-bottom: 20px; /* More space below the dropdown */
    appearance: none; /* Remove default system dropdown styling */
    -webkit-appearance: none;
    -moz-appearance: none;
    background-image: url('data:image/svg+xml;utf8,<svg fill="%23abb2bf" height="24" viewBox="0 0 24 24" width="24" xmlns="http://www.w3.org/2000/svg"><path d="M7 10l5 5 5-5z"/></svg>'); /* Custom arrow icon */
    background-repeat: no-repeat;
    background-position: right 10px center;
    background-size: 12px;
}

.algorithm-dropdown:focus {
    outline: none;
    border-color: #61afef; /* Highlight color on focus */
    box-shadow: 0 0 0 0.2rem rgba(97, 175, 239, 0.25); /* Subtle focus shadow */
}

.algorithm-dropdown option {
    background-color: #3b4048; /* Background for dropdown options */
    color: #abb2bf; /* Text color for options */
}

.pseudocode-block {
    margin-top: 15px;
    padding: 0; /* Remove padding here, let the code block handle it */
    background-color: transparent; /* No background needed here, code block has its own */
    border-radius: 8px;
    overflow-x: auto; /* Add scroll if code is too wide */
    position: relative; /* Needed for potential absolute positioning of copy buttons etc. */
}

/* Style for the initial message */
.pseudocode-block p {
    color: #bbb; /* Slightly muted text */
    font-style: italic;
    text-align: center;
    padding: 20px; /* Add padding to the message */
}

/* Styling for the actual code block within the display area */
.pseudocode-block pre {
    margin: 0; /* Remove default pre margin */
    padding: 15px; /* Add padding inside the code block */
    border-radius: 8px; /* Match container border-radius */
    background-color: #1e1e1e; /* Dark background for the code area */
    color: #abb2bf; /* Default code text color */
    font-family: 'Fira Code', 'Consolas', 'Monaco', 'Andale Mono', 'Ubuntu Mono', monospace; /* Monospaced font for code */
    font-size: 0.9em; /* Slightly smaller font size */
    line-height: 1.5;
    tab-size: 4;
    white-space: pre-wrap; /* Wrap long lines */
    word-wrap: break-word;
}

/* Pygments/Syntax Highlighting styles */
/* These styles are often provided by your MkDocs theme and Pygments.
   The colors below are examples that might resemble a VS Code dark theme.
   You might need to adjust these or rely on your theme's built-in styles.
   If the highlighting isn't working or the colors are wrong, check your
   mkdocs.yml theme configuration for Pygments.
*/
.pseudocode-block code .k { color: #c678dd; } /* Keyword */
.pseudocode-block code .nf { color: #61afef; } /* Function name */
.pseudocode-block code .nc { color: #e5c07b; } /* Class name */
.pseudocode-block code .s { color: #98c379; } /* String */
.pseudocode-block code .c1 { color: #5c6370; font-style: italic; } /* Comment */
.pseudocode-block code .o { color: #56b6c2; } /* Operator */
.pseudocode-block code .mi { color: #d19a66; } /* Integer */
.pseudocode-block code .mf { color: #d19a66; } /* Float */
.pseudocode-block code .bp { color: #e5c07b; } /* Built-in pseudo */
.pseudocode-block code .nv { color: #abb2bf; } /* Variable name */
/* Add more Pygments classes as needed based on inspection */

</style>