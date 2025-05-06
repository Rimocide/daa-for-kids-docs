# Longest Common Subsequence (LCS) Visualizer

Welcome to the **Longest Common Subsequence (LCS) Visualizer** module. This tool provides an interactive way to understand a fundamental problem in computer science and bioinformatics: finding the longest subsequence common to two sequences. While the concept might sound simple, the efficient solution often involves dynamic programming, which this visualizer helps illuminate.

---

## Understanding the Longest Common Subsequence

A subsequence is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements. For example, "ACE" is a subsequence of "ABCDE". The Longest Common Subsequence (LCS) of two sequences is the longest subsequence that is present in both sequences. Unlike substrings, the common characters in an LCS do not need to be consecutive in the original sequences.

The LCS problem has numerous applications, such as in file comparison utilities (like `diff`), bioinformatics for comparing DNA or protein sequences, and data compression.

---

## Exploring the Visualizer

The LCS Visualizer is designed to make the dynamic programming approach to solving the 0/1 Knapsack Problem transparent and interactive. The main interface, as seen in the **Visualization** tab, centers around the **DP Table Visualization**.

* **DP Table Visualization:** This is the core of the visualizer. It displays the dynamic programming table used to compute the length of the LCS and reconstruct the subsequence itself. Each cell in the table represents the length of the LCS for prefixes of the two input strings.
* **Interactive Steps:** The visualizer allows you to **manually traverse the steps** involved in filling the DP table. You can see how each cell's value is calculated based on the values of previous cells and whether the corresponding characters in the input strings match. This step-by-step process is crucial for understanding the dynamic programming recurrence relation.
* **Traceback:** Once the DP table is filled, the visualizer can often show the traceback path through the table that reveals the actual Longest Common Subsequence.

Beyond the main visualization, the page features additional tabs to deepen your understanding:

* **Algorithm Tab:** This section provides a clear and intuitive explanation of the LCS algorithm through text, breaking down the logic and concepts without relying solely on the visualization.
* **Applications Tab:** This tab details the various **use cases and real-world applications** of the Longest Common Subsequence problem across different fields, highlighting its practical importance.

---

## Algorithm Pseudocode

Understand the computational steps behind the Dynamic Programming approach to the LCS problem by reviewing its pseudocode.

<div class="algorithm-pseudocode-container">
    <select id="lcs-algorithm-selector" class="algorithm-dropdown">
        <option value="">Select an Algorithm</option>
        <option value="dynamic-programming">Dynamic Programming (LCS)</option>
    </select>

    <div id="lcs-pseudocode-display" class="pseudocode-block">
        <p>Select an algorithm from the dropdown above to see its pseudocode.</p>
    </div>

    <div id="dynamic-programming-pseudocode" class="pseudocode-content" style="display: none;">
        ```python
        LCS_Length(string_A, string_B):
            m = length of string_A
            n = length of string_B

            // Create a DP table dp of size (m+1) x (n+1)
            // dp[i][j] will store the length of LCS of string_A[0..i-1] and string_B[0..j-1]
            create a table dp of size (m+1) x (n+1)

            // Fill dp table in bottom-up manner
            for i from 0 to m:
                for j from 0 to n:
                    if i == 0 or j == 0:
                        dp[i][j] = 0
                    else if string_A[i-1] == string_B[j-1]:
                        dp[i][j] = dp[i-1][j-1] + 1
                    else:
                        dp[i][j] = max(dp[i-1][j], dp[i][j-1])

            // dp[m][n] contains the length of the LCS for string_A and string_B
            lcs_length = dp[m][n]

            // Optional: Reconstruct the LCS string
            lcs_string = ""
            i = m
            j = n
            while i > 0 and j > 0:
                if string_A[i-1] == string_B[j-1]:
                    lcs_string = string_A[i-1] + lcs_string // Prepend character
                    i = i - 1
                    j = j - 1
                elif dp[i-1][j] > dp[i][j-1]:
                    i = i - 1
                else:
                    j = j - 1

            return lcs_length, lcs_string
        ```
    </div>
</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
    const selector = document.getElementById('lcs-algorithm-selector'); // Unique ID for this selector
    const pseudocodeDisplay = document.getElementById('lcs-pseudocode-display'); // Unique ID for this display area
    // Selects hidden divs immediately following the display area with the specific class
    const pseudocodeContentBlocks = document.querySelectorAll('#lcs-pseudocode-display + .pseudocode-content');

    console.log("LCS Script loaded. Pseudocode content blocks found:", pseudocodeContentBlocks.length);

    selector.addEventListener('change', function() {
        const selectedAlgorithm = selector.value;
        console.log('Selected LCS algorithm:', selectedAlgorithm);

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