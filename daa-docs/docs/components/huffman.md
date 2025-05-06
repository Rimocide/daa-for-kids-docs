# Huffman Encoding Visualizer

Welcome to the **Huffman Encoding Visualizer** module, a tool designed to demystify the process of **data compression** using the Huffman coding algorithm. Understanding how data can be efficiently represented is a key concept in computer science, and this visualizer makes the principles of Huffman encoding clear and interactive.

---

## Understanding Huffman Encoding

Huffman Encoding is a widely used lossless data compression algorithm. It works by assigning variable-length codes to input characters, where the length of each code is inversely proportional to the frequency of the character. More frequent characters get shorter codes, while less frequent characters get longer codes. This results in a shorter average code length, leading to compression of the overall data.

The core idea is to build a binary tree (the Huffman Tree) based on the frequencies of characters in the input data. The path from the root to each character leaf node represents its unique binary code.

---

## Exploring the Visualizer

The Huffman Encoding Visualizer allows you to input text and see the entire encoding and decoding process unfold across different tabs.

### Visualization Tab

The **Visualization** tab is where you can see the fundamental components of the Huffman encoding process:

* **Huffman Tree:** Witness the construction of the Huffman tree based on the character frequencies of your input text. The tree visually represents how character codes are derived.
* **Character Frequencies:** View a table listing each unique character in your input text and its corresponding frequency count. This is the basis for building the Huffman tree.
* **Huffman Codes:** See the generated variable-length binary code for each character. Observe how characters with higher frequencies have shorter codes.

You can input your own text or select from **Sample Texts** to quickly explore different scenarios and see how the tree and codes change. The visualizer also immediately shows the **Original size** (in bits), the **Compressed size** (in bits), and the **Space saved** as a percentage, demonstrating the effectiveness of the compression.

### Encoding Tab

The **Encoding** tab focuses on the process of converting your original text into its compressed binary representation. This page details the step-by-step process of encoding the input string using the generated Huffman codes and displays the resulting **Encoded String**. You can see how the original characters are replaced by their shorter binary codes, leading to the reduction in data size.

### Decoding Tab

The **Decoding** tab allows you to take an encoded binary string and convert it back to the original text using the Huffman tree. This page demonstrates how the algorithm traverses the tree based on the binary code to reconstruct the original characters. You can also **enter strings to decode**, allowing you to experiment with the decoding process directly.

---

## Algorithm Pseudocode

Understand the computational steps involved in constructing the Huffman Tree and generating the codes.

<div class="algorithm-pseudocode-container">
    <select id="huffman-algorithm-selector" class="algorithm-dropdown">
        <option value="">Select an Algorithm</option>
        <option value="huffman-encoding">Huffman Encoding</option>
    </select>

    <div id="huffman-pseudocode-display" class="pseudocode-block">
        <p>Select an algorithm from the dropdown above to see its pseudocode.</p>
    </div>

    <div id="huffman-encoding-pseudocode" class="pseudocode-content" style="display: none;">
        ```python
        Huffman_Encoding(text):
            // 1. Calculate character frequencies
            create a frequency map of characters in the text

            // 2. Create priority queue of nodes
            create a min-priority queue PQ
            for each character and its frequency:
                create a leaf node with character and frequency
                PQ.add(node, frequency)

            // 3. Build the Huffman Tree
            while size of PQ > 1:
                // Extract the two nodes with the smallest frequencies
                left_child = PQ.extract_min()
                right_child = PQ.extract_min()

                // Create a new internal node with frequency equal to the sum of the two children
                // The new node's children are the extracted nodes
                new_frequency = left_child.frequency + right_child.frequency
                internal_node = create a node with new_frequency, left_child, right_child
                PQ.add(internal_node, new_frequency)

            // The remaining node in the PQ is the root of the Huffman Tree
            huffman_tree_root = PQ.extract_min()

            // 4. Generate Huffman Codes
            create a map to store character codes
            traverse the Huffman tree from the root:
                if current node is a leaf:
                    store the path from root to this leaf as the character's code
                else:
                    traverse left child, appending '0' to current path
                    traverse right child, appending '1' to current path

            // 5. Encode the original text
            encoded_string = ""
            for each character in the original text:
                append the character's Huffman code to encoded_string

            return encoded_string, huffman_tree_root, character_codes
        ```
    </div>
</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
    const selector = document.getElementById('huffman-algorithm-selector'); // Unique ID for this selector
    const pseudocodeDisplay = document.getElementById('huffman-pseudocode-display'); // Unique ID for this display area
    // Selects hidden divs immediately following the display area with the specific class
    const pseudocodeContentBlocks = document.querySelectorAll('#huffman-pseudocode-display + .pseudocode-content');

    console.log("Huffman Script loaded. Pseudocode content blocks found:", pseudocodeContentBlocks.length);

    selector.addEventListener('change', function() {
        const selectedAlgorithm = selector.value;
        console.log('Selected Huffman algorithm:', selectedAlgorithm);

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
/* Reusing styles from previous sections for consistency */
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