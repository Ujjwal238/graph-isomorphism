# Graph Isomorphism Checker

A comprehensive implementation of tree isomorphism detection using the center-finding and canonical labeling algorithm. This repository provides both C++ command-line tools and a web-based interactive interface for checking if two trees are isomorphic.

## 🔍 Overview

Tree isomorphism is a fundamental problem in graph theory where we determine if two trees have the same structural properties - essentially, if one tree can be transformed into another through relabeling of vertices. This implementation uses an efficient algorithm based on:

1. **Tree centering**: Finding the center(s) of each tree
2. **Canonical rooting**: Rooting trees at their centers
3. **Recursive labeling**: Creating unique labels for subtree structures
4. **Label comparison**: Comparing canonical labels to determine isomorphism

## 🚀 Features

### C++ Implementation
- **Efficient tree center finding** using iterative leaf removal
- **Canonical tree labeling** with hash-based subtree identification
- **Multiple center handling** for trees with 1 or 2 centers
- **File-based input/output** for batch processing
- **Debug output** showing adjacency lists, centers, and labels

### Web Interface
- **Interactive graph editor** with dual workspaces
- **Visual vertex and edge manipulation** using HTML5 Canvas
- **Real-time graph construction** with mouse interactions
- **Adjacency list export** for C++ processing
- **Responsive design** for different screen sizes

## 🛠️ Installation & Usage

### C++ Version

#### Prerequisites
- C++ compiler (GCC, Clang, or MSVC)
- C++11 or higher support

#### Compilation
```bash
# Compile the main version
g++ -o graph_iso code.cpp -std=c++11

# Or compile the string-label version
g++ -o graph_iso_test test.cpp -std=c++11
```

#### Running
```bash
# Ensure input.txt and input2.txt are in the same directory
./graph_iso

# The program will output:
# - Adjacency lists for both graphs
# - Tree centers for each graph
# - Labeled trees and comparison results
```

### Web Version

#### Local Setup
1. Clone the repository:
```bash
git clone https://github.com/Ujjwal238/graph-isomorphism.git
cd graph-isomorphism
```

2. Open `index.html` in a web browser
3. Use the interactive interface to create graphs
4. Export adjacency lists for C++ processing

#### Live Demo
Visit: [ujjwal238.github.io/graph-isomorphism/](https://ujjwal238.github.io/graph-isomorphism/)

## 📊 Input Format

The program accepts graphs in adjacency list format where each line represents a vertex and its connections:

```
vertex_id neighbor1 neighbor2 neighbor3 ...
```

### Example Input Files

**input.txt:**
```
1 5 7
2 7
3 5
4 6 9
5 1 3
6 4 12
7 8 2 1 11
8 9 7
9 4 8 10
10 9
11 7
12 6
```

**input2.txt:**
```
1 2
2 3 1 12
3 2 5 10
4 8 6 5 11
5 3 4
6 4
7 9
8 4 9
9 8 7
10 3
11 4
12 2
```

## 🔧 Algorithm Details

### Core Algorithm Flow

1. **Input Processing**
   - Read adjacency lists from input files
   - Build internal graph representation using `std::map<int, std::vector<int>>`

2. **Center Finding** (`findTreeCenters`)
   - Iteratively remove leaf nodes (degree ≤ 1)
   - Continue until 1 or 2 nodes remain
   - These remaining nodes are the tree centers

3. **Tree Rooting** (`root_tree`)
   - Root the tree at each center using BFS
   - Create directed adjacency list from root to children

4. **Canonical Labeling** (`label_tree`)
   - **code.cpp**: Uses integer hash labels for efficiency
   - **test.cpp**: Uses string concatenation for readability
   - Recursively compute labels bottom-up
   - Sort child labels for canonical ordering

5. **Isomorphism Check**
   - Compare labels from all possible center rootings
   - Handle cases with different numbers of centers
   - Return true if any rooting pair produces matching labels

### Key Functions

#### `readAdjacencyList(filename)`
```cpp
map<int, vector<int>> readAdjacencyList(const string& filename)
```
- Reads graph from file into adjacency list format
- Handles whitespace-separated neighbor lists
- Returns empty map on file errors

#### `findTreeCenters(adjList)`
```cpp
vector<int> findTreeCenters(const map<int, vector<int>>& adjList)
```
- Implements iterative leaf removal algorithm
- Time complexity: O(n) where n is number of vertices
- Returns 1 center for odd-diameter trees, 2 for even-diameter

#### `root_tree(adjList, root)`
```cpp
map<int, vector<int>> root_tree(map<int, vector<int>>& adjList, int root)
```
- Converts undirected tree to rooted tree using BFS
- Creates parent-to-children adjacency representation
- Preserves tree structure while establishing hierarchy

#### `label_tree(node, rootedAdjList)`
```cpp
int label_tree(int node, map<int, vector<int>>& rootedAdjList)  // code.cpp
string label_tree(int node, map<int, vector<int>>& rootedAdjList)  // test.cpp
```
- Recursively computes canonical subtree labels
- Sorts child labels for consistent ordering
- Uses hash mapping (code.cpp) or string concatenation (test.cpp)

## 🎮 Web Interface Usage

### Graph Creation
1. **Adding Vertices**: Click "Add Vertex" button or double-click empty canvas area
2. **Creating Edges**: Double-click a vertex to enter edge-drawing mode, then click target vertex
3. **Moving Vertices**: Click and drag vertices to reposition them
4. **Removing Vertices**: Click "Remove Vertex" then click the vertex to delete

### Canvas Interactions
- **Mouse Down**: Start vertex dragging or edge creation
- **Mouse Move**: Update vertex position while dragging
- **Mouse Up**: Complete dragging or enter edge-drawing mode
- **Double Click**: Select vertex for edge creation

### Export Functionality
- Click "Generate List" to download adjacency list file
- Files are named `input.txt` and `input2.txt` for direct C++ use
- Format matches the expected input format for the C++ programs

## 🔍 Example Analysis

### Sample Trees
The included test files represent two different tree structures:

**Tree 1 (input.txt)**: 12-vertex tree with center at vertex 7
**Tree 2 (input2.txt)**: 12-vertex tree with different structure

### Expected Output
```
Adjacency List 1:
1: 5 -> 7
2: 7
3: 5
...

Centers of Tree 1: 7
Centers of Tree 2: 2

Labeled Tree: [hash_value_1]
Labeled Tree: [hash_value_2]

TREES ARE ISOMORPHIC / NOT ISOMORPHIC
```

## 🧪 Testing

### Test Cases
1. **Identical Trees**: Should return "TREES ARE ISOMORPHIC"
2. **Different Structures**: Should return "NOT ISOMORPHIC"
3. **Single vs Double Centers**: Algorithm handles both cases
4. **Edge Cases**: Single vertex, two vertices, linear trees

### Validation
- Verify center finding with known tree structures
- Test label consistency across different rootings
- Confirm isomorphism transitivity and symmetry

## ⚡ Performance Analysis

### Time Complexity
- **Center Finding**: O(n) where n = number of vertices
- **Tree Rooting**: O(n) using BFS traversal
- **Labeling**: O(n log n) due to sorting child labels
- **Overall**: O(n log n) for most practical cases

### Space Complexity
- **Adjacency Lists**: O(n + m) where m = number of edges
- **Rooted Trees**: O(n) additional space
- **Label Storage**: O(n) for labels and hash maps
- **Overall**: O(n) for tree structures

## 🐛 Known Limitations

1. **Tree-Only**: Algorithm specifically designed for trees, not general graphs
2. **Integer Overflow**: Large hash values might cause overflow in code.cpp
3. **Memory Usage**: String concatenation in test.cpp can be memory-intensive
4. **File Dependencies**: Hard-coded input file names limit flexibility

## 🔮 Future Enhancements

### Algorithm Improvements
- **General Graph Isomorphism**: Extend to handle cycles and general graphs
- **Optimization**: Implement more efficient labeling schemes
- **Parallelization**: Add multi-threading for large tree processing

### Interface Enhancements
- **Graph Import**: Support multiple input formats (JSON, GraphML, etc.)
- **Visualization**: Add tree layout algorithms and better rendering
- **Real-time Analysis**: Live isomorphism checking during graph construction
- **Export Options**: Support various output formats and visualization

### Code Quality
- **Error Handling**: Improve robustness and error reporting
- **Configuration**: Add command-line arguments and config files
- **Testing**: Comprehensive unit test suite and benchmarking
- **Documentation**: Add code comments and API documentation

## 📚 Mathematical Background

### Tree Isomorphism Theory
Two trees T₁ and T₂ are isomorphic if there exists a bijection f: V(T₁) → V(T₂) such that (u,v) ∈ E(T₁) if and only if (f(u), f(v)) ∈ E(T₂).

### Center-Based Canonical Form
Every tree has either 1 or 2 centers. By rooting trees at their centers and creating canonical labels, we can determine isomorphism by comparing these canonical forms.

### Labeling Algorithm
The recursive labeling scheme ensures that:
- Identical subtree structures produce identical labels
- Different subtree structures produce different labels
- The complete tree label uniquely identifies the tree structure

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature-name`
3. Make your changes and test thoroughly
4. Commit with descriptive messages: `git commit -m "Add feature description"`
5. Push to your fork: `git push origin feature-name`
6. Create a Pull Request with detailed description

### Contribution Guidelines
- Follow existing code style and conventions
- Add tests for new features
- Update documentation for API changes
- Ensure backward compatibility when possible

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 👨‍💻 Author

**Ujjwal238**
- GitHub: [@Ujjwal238](https://github.com/Ujjwal238)
- Project: [graph-isomorphism](https://github.com/Ujjwal238/graph-isomorphism)

## 🙏 Acknowledgments

- Tree isomorphism algorithms from computer science literature
- HTML5 Canvas API for web interface implementation
- C++ Standard Library for efficient data structures
- Community feedback and testing contributions

---

*For questions, bug reports, or feature requests, please open an issue on the GitHub repository.*
