# Knapsack Problem Visualizer

Welcome to the **Knapsack Problem Visualizer** module, where you can explore the classic **0/1 Knapsack Problem** through interactive visualizations. This module is designed to make understanding optimization problems and the power of dynamic programming both accessible and engaging.

---

## Understanding the Knapsack Problem

The 0/1 Knapsack Problem is a fundamental problem in combinatorial optimization. Imagine you have a knapsack with a maximum weight capacity and a set of items, each with a specific weight and value. The problem is to determine which items to include in the knapsack such that the total weight does not exceed the capacity, and the total value is maximized. The "0/1" signifies that you can either take an item entirely (1) or not take it at all (0) – you cannot take a fraction of an item.

Our visualizer helps you grasp the core concepts and different approaches to solving this problem.

---

## Problem Setup

The journey begins on the **Problem Setup** page. Here, you define the parameters of your specific Knapsack problem:

- **Bag Constraints:** Set the maximum weight capacity your knapsack can hold. You can also specify an empty bag weight if relevant to your scenario.
- **Add New Item:** Define the items available to be potentially placed in the knapsack. For each item, you specify its name, weight, and monetary worth. You have the flexibility to add individual items or generate sets of items with varying characteristics (heavy, light, or mixed).

This interactive setup allows you to create custom problem instances to test different scenarios and see how algorithms perform.

---

## Solving the Problem

Once your problem is defined, you move to the **Solve Problem** section. Here, you choose the algorithm you want to use to find the optimal solution. While the visualizer may offer multiple approaches, the **Dynamic Programming** method is a central focus.

The dynamic programming approach to the Knapsack Problem is particularly insightful. It builds a solution iteratively by considering smaller subproblems. The visualizer beautifully showcases this process by constructing the **Dynamic Programming Table**. This table systematically records the maximum value that can be obtained for different capacities and subsets of items. The visualizer allows you to **manually traverse the steps** of filling this table, providing a clear, step-by-step understanding of how the optimal solution is derived from the solutions to subproblems. This interactive exploration of the DP table is key to demystifying this powerful technique.

---

## Viewing Results and Comparison

The **View Results** page presents the outcome of the chosen algorithm. You can see which items were selected for the knapsack, the total weight, and the total value achieved.

A valuable feature of this module is the ability to **Compare Both Algorithms** (typically Dynamic Programming and the Greedy approach). The visualizer will run both algorithms on your defined problem instance and present their respective results side-by-side.

Furthermore, an integrated **AI Assistant** is available throughout the module. On the comparison page, the AI Assistant provides insights into the performance of each algorithm for your specific problem instance, often highlighting why one might have performed better than the other, especially in cases where the Greedy approach fails to find the globally optimal solution that Dynamic Programming guarantees. This feature adds an extra layer of guidance and explanation to enhance your learning.

---

## Algorithm Pseudocode

Explore the fundamental logic behind the Greedy and Dynamic Programming approaches to the Knapsack Problem. Select an algorithm from the dropdown below to view its pseudocode.

<div class="algorithm-pseudocode-container">
    <select id="knapsack-algorithm-selector" class="algorithm-dropdown">
        <option value="">Select an Algorithm</option>
        <option value="greedy">Greedy Approach</option>
        <option value="dynamic-programming">Dynamic Programming (0/1 Knapsack)</option>
    </select>

    <div id="knapsack-pseudocode-display" class="pseudocode-block">
        <p>Select an algorithm from the dropdown above to see its pseudocode.</p>
    </div>

    <div id="greedy-pseudocode" class="pseudocode-content" style="display: none;">
        ```python
        Greedy_Knapsack(items, capacity):
            // items is a list of (value, weight) tuples
            // capacity is the maximum weight the knapsack can hold

            sort items by value-to-weight ratio in descending order

            current_weight = 0
            total_value = 0
            knapsack_items = []

            for each item (value, weight) in sorted items:
                if current_weight + weight <= capacity:
                    knapsack_items.add(item)
                    current_weight = current_weight + weight
                    total_value = total_value + value
                // In 0/1 Knapsack, we cannot take a fraction, so we skip if it doesn't fit

            return total_value, knapsack_items
        ```
    </div>

    <div id="dynamic-programming-pseudocode" class="pseudocode-content" style="display: none;">
        ```python
        Dynamic_Programming_Knapsack(items, capacity):
            // items is a list of (value, weight) tuples
            // capacity is the maximum weight the knapsack can hold
            // n is the number of items

            n = number of items
            create a DP table K of size (n+1) x (capacity+1)

            // Build the DP table K[][]
            for i from 0 to n:
                for w from 0 to capacity:
                    if i == 0 or w == 0:
                        K[i][w] = 0
                    else if items[i-1].weight <= w:
                        // Max of including item i or excluding item i
                        K[i][w] = max(items[i-1].value + K[i-1][w - items[i-1].weight], K[i-1][w])
                    else:
                        // Item i cannot be included
                        K[i][w] = K[i-1][w]

            // The maximum value is in K[n][capacity]
            max_value = K[n][capacity]

            // Reconstruct the items included (optional, but useful)
            included_items = []
            w = capacity
            for i from n down to 1:
                if max_value <= 0:
                    break // Knapsack is full or no more value to add

                // If the current value is NOT the same as the value excluding item i,
                // then item i must have been included.
                if max_value != K[i-1][w]:
                    included_items.add(items[i-1])
                    max_value = max_value - items[i-1].value
                    w = w - items[i-1].weight

            return K[n][capacity], included_items
        ```
    </div>

</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
    const selector = document.getElementById('knapsack-algorithm-selector'); // Unique ID for this selector
    const pseudocodeDisplay = document.getElementById('knapsack-pseudocode-display'); // Unique ID for this display area
    const pseudocodeContentBlocks = document.querySelectorAll('#knapsack-pseudocode-display + .pseudocode-content'); // Selects hidden divs immediately following the display area

    console.log("Knapsack Script loaded. Pseudocode content blocks found:", pseudocodeContentBlocks.length);

    selector.addEventListener('change', function() {
        const selectedAlgorithm = selector.value;
        console.log('Selected Knapsack algorithm:', selectedAlgorithm);

        // Hide all pseudocode blocks
        pseudocodeContentBlocks.forEach(block => {
            block.style.display = 'none';
        });

        // Clear the main display area
        pseudocodeDisplay.innerHTML = '';

        if (selectedAlgorithm) {
            const targetBlockId = selectedAlgorithm + '-pseudocode';
            console.log('Looking for target block with ID:', targetBlockId);
            // Find the target block by ID within the same container or document
            const targetBlock = document.getElementById(targetBlockId);


            if (targetBlock) {
                console.log('Target block FOUND:', targetBlock);
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
                console.error('Could not find target block with ID:', targetBlockId);
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
/* Reusing styles from the Pathfinding section for consistency */
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
    font-size: 0.7rem;
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
    border-color:rgb(207, 93, 224); /* Highlight color on focus */
    box-shadow: 0 0 0 0.2rem rgba(214, 138, 226, 0.25); /* Subtle focus shadow */
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

/* Pygments/Syntax Highlighting styles - Ensure these are consistent or rely on your theme */
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

---

By utilizing the Knapsack Problem Visualizer, you can gain a deep, intuitive understanding of this classic optimization challenge, appreciate the elegance of dynamic programming, and see the practical differences between different algorithmic strategies.
