# Project Report: SkillMash - A Smart Job Candidate Ranking System

## Table of Content
1. INTRODUCTION & PROBLEM
2. PARADIGMS
3. ALGORITHM & EXPLANATION
4. ALGORITHM CODE REFERENCE
5. INTERFACES
6. CONCLUSION

---

## 1. INTRODUCTION & PROBLEM

### Introduction
**SkillMash** is a comprehensive software solution designed to automate and optimize the process of shortlisting candidates for job openings. In the modern recruitment landscape, parsing hundreds of resumes and manually ranking candidates based on skills, experience, and salary expectations is time-consuming and prone to bias.

### Problem Statement
Recruiters face significant challenges in:
- Efficiently searching and filtering candidates from large datasets.
- Objectively measuring the "fit" between a candidate's skills and a job description.
- Analyzing relationships between different skills (e.g., knowing "React" implies familiarity with "JavaScript").
- Selecting the best set of candidates within a limited hiring budget.

This project addresses these problems by implementing a suite of efficient Data Structures and Algorithms (DSA) to handle parsing, searching, ranking, and selecting candidates effectively.

---

## 2. PARADIGMS

The project utilizes several algorithmic paradigms to solve specific sub-problems efficiently.


### Divide and Conquer
This paradigm is used to break down complex problems into smaller sub-problems.
- **Merge Sort**: Recursively divides the list of candidates into halves, sorts them, and merges the sorted halves.
- **Quick Sort**: Partitions the list around a pivot and recursively sorts the sub-lists.

### Dynamic Programming (DP)
DP is used to solve optimization problems by breaking them down into simpler overlapping sub-problems.
- **Longest Common Subsequence (LCS)**: Used to calculate a "Fit Score" by finding the longest sequence of skills that appear in both the job description and the candidate's profile, optimizing the match quality metric.

### Greedy Approach
This paradigm makes the locally optimal choice at each stage with the hope of finding a global optimum.
- **Greedy Selection**: Used for selecting the maximum number of high-quality candidates that fit within a specific salary budget.

### Transform and Conquer
- **Max Heap**: Transforms the list of candidates into a Heap data structure to efficiently extract the top-ranked candidates (Priority Queue).
- **AVL Tree**: Transforms a standard Binary Search Tree into a balanced one to ensure $O(\log n)$ search times.

### Space-Time Tradeoff
- **Trie (Prefix Tree)**: Uses additional memory to store strings character-by-character to enable ultra-fast prefix lookups and auto-completion.
- **Hash Indexing**: Uses a Hash Map to trade space for $O(1)$ average time complexity when looking up skills or candidates.

---

## 3. ALGORITHM & EXPLANATION

The following algorithms have been implemented to power SkillMash:

### 1. Merge Sort & Quick Sort
- **Purpose**: To order candidates based on specific criteria such as **Expected Salary** (Ascending) or **Experience Years** (Descending).
- **Mechanism**: 
  - *Merge Sort* guarantees stable $O(n \log n)$ performance.
  - *Quick Sort* provides high performance on average for in-memory lists.

### 2. Knuth-Morris-Pratt (KMP) Algorithm
- **Purpose**: To efficiently search for specific keywords or patterns within large blocks of text (e.g., searching for a specific technology in a candidate's resume/bio).
- **Mechanism**: Preprocesses the pattern to create a "Longest Prefix Suffix" (LPS) array, allowing the search to skip unnecessary comparisons upon mismatch, achieving $O(n+m)$ complexity.

### 3. Dynamic Programming (LCS)
- **Purpose**: To compute a **Fit Score** quantifying the similarity between a candidate's skill set and the job requirements.
- **Mechanism**: Constructs a DP matrix to find the length of the Longest Common Subsequence between two skill lists. The score is derived from the percentage of required skills matched.

### 4. Graph Construction (Adjacency List)
- **Purpose**: To model relationships between skills. If two skills frequently appear together in candidate profiles, they are linked.
- **Mechanism**: Uses an Adjacency List where each skill points to related skills with a weight representing co-occurrence frequency. This allows the system to suggest "Related Skills" (e.g., searching "C#" suggests ".NET").

### 5. Max Heap (Priority Queue)
- **Purpose**: To efficiently retrieve the Top-N candidates based on experience or score without sorting the entire dataset.
- **Mechanism**: strict binary heap structure where the parent node is always greater than or equal to its children. Extracting the max element takes $O(\log n)$.

### 6. Trie (Prefix Tree)
- **Purpose**: Fast keyword insertion and efficient prefix-based searching.
- **Mechanism**: Stores characters of strings as nodes in a tree data structure. This is ideal for features like **Autocomplete** or checking if a skill exists in the database.

### 7. AVL Tree (Self-Balancing BST)
- **Purpose**: To maintain a sorted dynamic list of candidates or skills that supports fast insertions, deletions, and lookups ($O(\log n)$).
- **Mechanism**: A Binary Search Tree that automatically rotates nodes (Left, Right, LR, RL) upon insertion to keep the height difference between left and right subtrees (balance factor) no more than 1.

### 8. Greedy Selection Algorithm
- **Purpose**: Portfolio/Team selection under a budget.
- **Mechanism**: Sorts candidates by a heuristic (e.g., Experience) and iterates through them, adding them to the selected set if their salary fits in the remaining budget.

### 9. Hash Indexing
- **Purpose**: Instant lookup of candidate details or skill IDs.
- **Mechanism**: Uses a Hash Table to map Keys (IDs/Names) to Values (Objects) for $O(1)$ access time.

---

## 4. ALGORITHM CODE

### 1. Merge Sort
```csharp
private void MergeSort(List<Candidate> list, int left, int right, AlgorithmTrace trace, ref int stepId)
{
    if (left < right)
    {
        int mid = (left + right) / 2;
        MergeSort(list, left, mid, trace, ref stepId);
        MergeSort(list, mid + 1, right, trace, ref stepId);
        Merge(list, left, mid, right, trace, ref stepId);
    }
}
```

### 2. Quick Sort
```csharp
private int Partition(List<Candidate> list, int low, int high, AlgorithmTrace trace, ref int stepId)
{
    var pivot = list[high];
    int i = (low - 1);
    for (int j = low; j < high; j++)
    {
        if (list[j].ExperienceYears > pivot.ExperienceYears) 
        {
            i++;
            Swap(list, i, j);
        }
    }
    Swap(list, i + 1, high);
    return i + 1;
}
```

### 3. KMP Algorithm (Pattern Search)
```csharp
public AlgorithmTrace SearchPattern(string text, string pattern)
{
    // ... setup ...
    int[] lps = ComputeLPS(pattern, trace, ref stepId);
    while (i < text.Length)
    {
        if (pattern[j] == text[i]) { j++; i++; }
        if (j == pattern.Length) { /* Found */ j = lps[j - 1]; }
        else if (i < text.Length && pattern[j] != text[i])
        {
            if (j != 0) j = lps[j - 1];
            else i++;
        }
    }
    return trace;
}
```

### 4. Dynamic Programming (Fit Score)
```csharp
public (double score, AlgorithmTrace trace) CalculateFitScore(List<string> jobSkills, List<string> candidateSkills)
{
    int m = jobSkills.Count;
    int n = candidateSkills.Count;
    int[,] dp = new int[m + 1, n + 1];

    for (int i = 0; i <= m; i++)
    {
        for (int j = 0; j <= n; j++)
        {
            if (i == 0 || j == 0) dp[i, j] = 0;
            else if (jobSkills[i - 1] == candidateSkills[j - 1])
                dp[i, j] = dp[i - 1, j - 1] + 1;
            else
                dp[i, j] = Math.Max(dp[i - 1, j], dp[i, j - 1]);
        }
    }
    return (m > 0 ? (double)dp[m, n] / m * 100 : 0, trace);
}
```

### 5. Graph Construction
```csharp
public AlgorithmTrace BuildSkillGraph(List<Candidate> candidates)
{
    foreach (var c in candidates)
    {
        var skills = c.CandidateSkills.Select(s => s.Skill.SkillName).ToList();
        for (int i = 0; i < skills.Count; i++)
        {
            for (int j = i + 1; j < skills.Count; j++)
            {
                AddEdge(skills[i], skills[j]);
                AddEdge(skills[j], skills[i]); // Undirected
            }
        }
    }
    return trace;
}
```

### 6. Max Heap
```csharp
private void Heapify(List<Candidate> heap, int n, int i, AlgorithmTrace trace, ref int stepCounter)
{
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;

    if (left < n && heap[left].ExperienceYears > heap[largest].ExperienceYears)
        largest = left;
    if (right < n && heap[right].ExperienceYears > heap[largest].ExperienceYears)
        largest = right;

    if (largest != i)
    {
        Swap(heap, i, largest);
        Heapify(heap, n, largest, trace, ref stepCounter);
    }
}
```

### 7. Trie (Prefix Tree)
```csharp
public void Insert(string word, AlgorithmTrace trace, ref int stepId)
{
    var node = _root;
    foreach (var ch in word.ToLower())
    {
        if (!node.Children.ContainsKey(ch))
            node.Children[ch] = new TrieNode();
        node = node.Children[ch];
    }
    node.IsEndOfWord = true;
}
```

### 8. AVL Tree
```csharp
private AVLNode Insert(AVLNode node, Candidate key)
{
    if (node == null) return new AVLNode(key);
    if (key.Id < node.Data.Id) node.Left = Insert(node.Left, key);
    else if (key.Id > node.Data.Id) node.Right = Insert(node.Right, key);
    else return node;

    node.Height = 1 + Math.Max(GetHeight(node.Left), GetHeight(node.Right));
    int balance = GetBalance(node);

    // Rotations
    if (balance > 1 && key.Id < node.Left.Data.Id) return RightRotate(node);
    if (balance < -1 && key.Id > node.Right.Data.Id) return LeftRotate(node);
    if (balance > 1 && key.Id > node.Left.Data.Id) { node.Left = LeftRotate(node.Left); return RightRotate(node); }
    if (balance < -1 && key.Id < node.Right.Data.Id) { node.Right = RightRotate(node.Right); return LeftRotate(node); }

    return node;
}
```

### 9. Greedy Selection
```csharp
public (List<Candidate> selected, AlgorithmTrace trace) SelectCandidates(List<Candidate> candidates, decimal budget)
{
    // Greedy Heuristic: Sort by Experience Descending
    var sortedCandidates = candidates.OrderByDescending(c => c.ExperienceYears).ToList();
    var selected = new List<Candidate>();
    decimal currentCost = 0;

    foreach (var c in sortedCandidates)
    {
        if (currentCost + c.ExpectedSalary <= budget)
        {
            selected.Add(c);
            currentCost += c.ExpectedSalary;
        }
    }
    return (selected, trace);
}
```

### 10. Hash Indexing
```csharp
private int GetBucketIndex(string key)
{
    int hash = Math.Abs(key.GetHashCode());
    return hash % _size;
}
// Chaining collision resolution handled in BuildIndex...
```

---

## 5. INTERFACES

The application uses defined models and structures to ensure clean interaction between the frontend, controllers, and algorithmic services.

### Data Models
- **`Candidate`**: Represents a job applicant with properties like `Id`, `FullName`, `ExperienceYears`, `ExpectedSalary`, and `CandidateSkills`.
- **`Skill`**: Represents a technical skill with `SkillId` and `SkillName`.
- **`AlgorithmTrace`**: A standardized object returned by all algorithm services to visualize the execution steps on the frontend. It contains a list of `AlgorithmStep`s.

### Service Interactions
- **Input**: Most services accept standard C# collections (`List<Candidate>`, `List<string>`) or primitives (`string pattern`, `decimal budget`).
- **Output**: Services return a tuple containing the Result (e.g., `List<Candidate>`, `double score`) and the `AlgorithmTrace` for visualization.

---

## 6. CONCLUSION

SkillMash successfully integrates advanced algorithmic concepts to solve real-world recruitment challenges. By leveraging algorithms like **DP**, **Graph Theory**, **KMP**, and **Sorting**, the system achieves both performance and accuracy. The use of visualization traces further enhances the system by allowing users to understand the "how" behind the ranking and selection logic.
