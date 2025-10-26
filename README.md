
# AI-Driven Personalized Learning System - Phase 1

## Project Overview
An adaptive learning system that creates personalized study plans based on learner profiles and a knowledge graph of prerequisite relationships between concepts.

## Project Structure

```
ai-learning-system/
├── README.md                    # This file
├── requirements.txt             # Python dependencies
├── main.py                      # Main entry point - orchestrates the entire system
├── src/
│   ├── __init__.py
│   ├── profile_manager.py       # Handles learner profile creation and loading
│   ├── knowledge_graph.py       # Builds and queries the knowledge graph
│   ├── planner_engine.py        # Generates personalized study plans
│   └── visualizer.py            # Creates visual outputs (graphs, plans)
├── data/
│   ├── sample_learners.json     # Sample learner profiles (created by script)
│   ├── concepts.json            # Learning concepts and prerequisites
│   └── resources.json           # Learning resources mapped to concepts
└── outputs/
    ├── knowledge_graph.png      # Visual representation of the graph
    └── study_plans/             # Generated study plans for each learner
```

---

## Installation and Setup

### Prerequisites
- Python 3.8 or higher
- pip (Python package installer)

### Installation Steps
```
# 1. Clone or download this project
# 2. Navigate to project directory
cd ai-learning-system

# 3. Install required packages
pip install -r requirements.txt
```

### requirements.txt Contents
```
networkx>=2.8
matplotlib>=3.5
```

---

## Module Documentation

### 1. profile_manager.py
**Purpose:** Manages learner profiles - creation, validation, loading, and saving.

**Functions to implement:**

```
def create_learner_profile(user_id: int, name: str, skill_level: str, 
                          learning_goal: str, preferred_style: str, 
                          completed_concepts: list = None) -> dict:
    """
    Creates a new learner profile dictionary.
    
    Args:
        user_id (int): Unique identifier for the learner
        name (str): Learner's name
        skill_level (str): One of ['Beginner', 'Intermediate', 'Advanced']
        learning_goal (str): What the learner wants to achieve
        preferred_style (str): One of ['Videos', 'Reading', 'Interactive', 'Mixed']
        completed_concepts (list): List of concept names already mastered (default: empty list)
    
    Returns:
        dict: A structured learner profile
        
    Example:
        profile = create_learner_profile(
            user_id=1,
            name="Alice",
            skill_level="Beginner",
            learning_goal="Learn Python Basics",
            preferred_style="Videos",
            completed_concepts=[]
        )
    """
    pass


def save_profile_to_json(profile: dict, filepath: str = "data/sample_learners.json"):
    """
    Saves a learner profile to a JSON file.
    Appends to existing file if it exists, creates new file otherwise.
    
    Args:
        profile (dict): The learner profile dictionary
        filepath (str): Path to save the JSON file
    
    Returns:
        bool: True if successful, False otherwise
    """
    pass


def load_profiles_from_json(filepath: str = "data/sample_learners.json") -> list:
    """
    Loads all learner profiles from a JSON file.
    
    Args:
        filepath (str): Path to the JSON file
    
    Returns:
        list: List of learner profile dictionaries
        
    Raises:
        FileNotFoundError: If the file doesn't exist
    """
    pass


def collect_profile_via_cli() -> dict:
    """
    Interactive CLI to collect learner profile information from user input.
    Asks questions and validates responses.
    
    Returns:
        dict: Complete learner profile
        
    Example interaction:
        Enter your name: Alice
        Select skill level (Beginner/Intermediate/Advanced): Beginner
        What is your learning goal?: Learn Python Basics
        Preferred learning style (Videos/Reading/Interactive/Mixed): Videos
    """
    pass
```

---

### 2. knowledge_graph.py
**Purpose:** Builds and manages the knowledge graph representing concepts and prerequisites.

**Functions to implement:**

```
import networkx as nx

def create_knowledge_graph(concepts: list, prerequisites: list) -> nx.DiGraph:
    """
    Creates a directed graph representing learning concepts and their prerequisites.
    
    Args:
        concepts (list): List of concept names (strings)
        prerequisites (list): List of tuples (prerequisite, dependent)
                             e.g., [("Variables", "Data Types"), ("Loops", "Functions")]
    
    Returns:
        nx.DiGraph: A NetworkX directed graph object
        
    Example:
        concepts = ["Variables", "Data Types", "Loops", "Functions"]
        prereqs = [("Variables", "Data Types"), ("Data Types", "Functions")]
        graph = create_knowledge_graph(concepts, prereqs)
    """
    pass


def load_concepts_from_json(filepath: str = "data/concepts.json") -> tuple:
    """
    Loads concepts and prerequisites from a JSON file.
    
    Expected JSON structure:
    {
        "concepts": ["Variables", "Data Types", "Loops", "Functions"],
        "prerequisites": [
            {"from": "Variables", "to": "Data Types"},
            {"from": "Data Types", "to": "Functions"}
        ]
    }
    
    Args:
        filepath (str): Path to concepts JSON file
    
    Returns:
        tuple: (concepts_list, prerequisites_list)
    """
    pass


def get_learning_sequence(graph: nx.DiGraph) -> list:
    """
    Returns the optimal learning sequence using topological sort.
    
    Args:
        graph (nx.DiGraph): The knowledge graph
    
    Returns:
        list: Ordered list of concepts to learn
        
    Raises:
        NetworkXError: If graph has cycles (invalid prerequisite structure)
    """
    pass


def get_concepts_for_skill_level(graph: nx.DiGraph, skill_level: str, 
                                 completed_concepts: list = None) -> list:
    """
    Filters concepts based on learner's skill level and completed concepts.
    
    Args:
        graph (nx.DiGraph): The knowledge graph
        skill_level (str): One of ['Beginner', 'Intermediate', 'Advanced']
        completed_concepts (list): Concepts already mastered by learner
    
    Returns:
        list: Filtered list of relevant concepts to learn
        
    Logic:
        - Beginner: Start from concepts with no prerequisites
        - Intermediate: Skip first 30% of total concepts
        - Advanced: Skip first 60% of total concepts
        - Always exclude already completed concepts
    """
    pass


def find_next_concepts(graph: nx.DiGraph, completed_concepts: list) -> list:
    """
    Finds the next concepts a learner should study based on completed concepts.
    
    Args:
        graph (nx.DiGraph): The knowledge graph
        completed_concepts (list): Concepts already mastered
    
    Returns:
        list: Concepts whose prerequisites are all completed
        
    Example:
        If learner completed ["Variables", "Data Types"],
        and "Functions" requires both, then "Functions" is returned
    """
    pass
```

---

### 3. planner_engine.py
**Purpose:** Generates personalized study plans by combining learner profile and knowledge graph.

**Functions to implement:**

```
def generate_study_plan(learner_profile: dict, knowledge_graph: nx.DiGraph) -> dict:
    """
    Creates a personalized study plan for a learner.
    
    Args:
        learner_profile (dict): Learner's profile from profile_manager
        knowledge_graph (nx.DiGraph): The knowledge graph
    
    Returns:
        dict: Study plan with structure:
        {
            "user_id": int,
            "name": str,
            "plan": [
                {"step": 1, "concept": "Variables", "status": "Not Started"},
                {"step": 2, "concept": "Data Types", "status": "Not Started"}
            ],
            "estimated_weeks": int,
            "learning_goal": str
        }
        
    Logic:
        1. Get learner's skill level and completed concepts
        2. Filter relevant concepts from graph
        3. Generate learning sequence
        4. Estimate time (1 week per 2-3 concepts for beginners)
    """
    pass


def estimate_learning_time(num_concepts: int, skill_level: str) -> int:
    """
    Estimates learning time in weeks based on number of concepts and skill level.
    
    Args:
        num_concepts (int): Total number of concepts to learn
        skill_level (str): Learner's skill level
    
    Returns:
        int: Estimated weeks to complete
        
    Calculation:
        - Beginner: 1 week per 2 concepts
        - Intermediate: 1 week per 3 concepts
        - Advanced: 1 week per 4 concepts
    """
    pass


def save_study_plan(study_plan: dict, output_dir: str = "outputs/study_plans/"):
    """
    Saves a study plan to a text file and JSON file.
    
    Args:
        study_plan (dict): The generated study plan
        output_dir (str): Directory to save files
        
    Creates two files:
        - outputs/study_plans/user_{id}_plan.json (machine-readable)
        - outputs/study_plans/user_{id}_plan.txt (human-readable)
    """
    pass


def format_plan_for_display(study_plan: dict) -> str:
    """
    Formats a study plan into a nicely formatted string for CLI display.
    
    Args:
        study_plan (dict): The study plan
    
    Returns:
        str: Formatted multi-line string ready to print
        
    Example output:
        ========================================
        PERSONALIZED STUDY PLAN
        ========================================
        Learner: Alice (ID: 1)
        Goal: Learn Python Basics
        Skill Level: Beginner
        
        Your Learning Path:
        1. ✓ Variables
        2. → Data Types
        3. → Loops
        4. → Functions
        
        Estimated completion: 4 weeks
        ========================================
    """
    pass
```

---

### 4. visualizer.py
**Purpose:** Creates visual representations of the knowledge graph and study plans.

**Functions to implement:**

```
import matplotlib.pyplot as plt
import networkx as nx

def visualize_knowledge_graph(graph: nx.DiGraph, 
                              output_path: str = "outputs/knowledge_graph.png",
                              highlight_concepts: list = None):
    """
    Creates a visual diagram of the knowledge graph.
    
    Args:
        graph (nx.DiGraph): The knowledge graph to visualize
        output_path (str): Where to save the image
        highlight_concepts (list): Concepts to highlight (e.g., completed ones)
    
    Returns:
        None (saves image to file)
        
    Visual specifications:
        - Use hierarchical layout (spring_layout or kamada_kawai_layout)
        - Node color: lightblue (default), lightgreen (highlighted)
        - Node size: 3000
        - Font size: 10
        - Show arrows for prerequisite direction
        - Include legend
    """
    pass


def create_progress_chart(learner_profile: dict, study_plan: dict,
                          output_path: str = "outputs/progress_chart.png"):
    """
    Creates a bar chart showing learning progress.
    
    Args:
        learner_profile (dict): The learner's profile
        study_plan (dict): Their current study plan
        output_path (str): Where to save the chart
    
    Returns:
        None (saves image to file)
        
    Chart shows:
        - X-axis: Concept names
        - Y-axis: Completion status (0 = Not Started, 1 = Completed)
        - Green bars for completed, gray for not started
    """
    pass
```

---

### 5. main.py
**Purpose:** Main entry point that orchestrates all components together.

**Integration Flow:**

```
"""
AI-Driven Personalized Learning System - Main Entry Point

This script ties together all components to create the complete system.

Execution Flow:
1. Load or create learner profile (profile_manager)
2. Load knowledge graph from data (knowledge_graph)
3. Generate personalized study plan (planner_engine)
4. Display plan to user (planner_engine)
5. Save outputs (planner_engine, visualizer)
6. Create visualizations (visualizer)
"""

def main():
    """
    Main function that runs the complete system.
    
    Steps:
        1. Welcome message and mode selection
        2. Profile handling:
           - Option A: Load existing profile
           - Option B: Create new profile via CLI
        3. Load knowledge graph from data/concepts.json
        4. Generate personalized study plan
        5. Display plan in CLI
        6. Save plan to files
        7. Generate and save visualizations
        8. Success message with output locations
    
    User Interaction:
        - Interactive CLI prompts
        - Clear feedback at each step
        - Error handling with helpful messages
    """
    pass


def display_welcome_banner():
    """
    Displays an ASCII art welcome banner for the system.
    """
    pass


def display_menu() -> str:
    """
    Shows menu options and returns user choice.
    
    Returns:
        str: User's menu selection ('1', '2', or '3')
        
    Options:
        1. Create new learner profile
        2. Load existing profile
        3. Exit
    """
    pass


if __name__ == "__main__":
    main()
```

**Integration Steps in main():**

```
# STEP 1: Import all modules
from src.profile_manager import collect_profile_via_cli, load_profiles_from_json, save_profile_to_json
from src.knowledge_graph import load_concepts_from_json, create_knowledge_graph
from src.planner_engine import generate_study_plan, format_plan_for_display, save_study_plan
from src.visualizer import visualize_knowledge_graph

# STEP 2: Get or create learner profile
choice = display_menu()
if choice == '1':
    learner_profile = collect_profile_via_cli()  # Creates new profile
    save_profile_to_json(learner_profile)  # Saves it
elif choice == '2':
    profiles = load_profiles_from_json()  # Loads existing
    learner_profile = profiles  # Select one (add selection logic)

# STEP 3: Load knowledge graph data
concepts, prerequisites = load_concepts_from_json("data/concepts.json")

# STEP 4: Create the knowledge graph object
knowledge_graph = create_knowledge_graph(concepts, prerequisites)

# STEP 5: Generate personalized study plan
study_plan = generate_study_plan(learner_profile, knowledge_graph)

# STEP 6: Display the plan
formatted_plan = format_plan_for_display(study_plan)
print(formatted_plan)

# STEP 7: Save outputs
save_study_plan(study_plan, "outputs/study_plans/")

# STEP 8: Create visualizations
visualize_knowledge_graph(
    knowledge_graph, 
    output_path="outputs/knowledge_graph.png",
    highlight_concepts=learner_profile.get("completed_concepts", [])
)

print("\n✓ Study plan saved to outputs/study_plans/")
print("✓ Knowledge graph saved to outputs/knowledge_graph.png")
```

---

## Data File Formats

### data/concepts.json
```
{
  "subject": "Python Programming Basics",
  "concepts": [
    "Variables",
    "Data Types",
    "Operators",
    "Conditionals",
    "Loops",
    "Functions",
    "Lists",
    "Dictionaries"
  ],
  "prerequisites": [
    {"from": "Variables", "to": "Data Types"},
    {"from": "Data Types", "to": "Operators"},
    {"from": "Operators", "to": "Conditionals"},
    {"from": "Conditionals", "to": "Loops"},
    {"from": "Data Types", "to": "Lists"},
    {"from": "Lists", "to": "Dictionaries"},
    {"from": "Loops", "to": "Functions"},
    {"from": "Dictionaries", "to": "Functions"}
  ]
}
```

### data/sample_learners.json
```
[
  {
    "user_id": 1,
    "name": "Alice",
    "skill_level": "Beginner",
    "learning_goal": "Learn Python Basics",
    "preferred_style": "Videos",
    "completed_concepts": []
  },
  {
    "user_id": 2,
    "name": "Bob",
    "skill_level": "Intermediate",
    "learning_goal": "Master Python Fundamentals",
    "preferred_style": "Reading",
    "completed_concepts": ["Variables", "Data Types"]
  }
]
```

---

## Running the System

### Basic Usage
```
python main.py
```

### Expected Output
```
========================================
AI-DRIVEN PERSONALIZED LEARNING SYSTEM
========================================

Select an option:
1. Create new learner profile
2. Load existing profile
3. Exit

Your choice: 1

[Profile collection via CLI...]

Generating personalized study plan...

========================================
YOUR PERSONALIZED STUDY PLAN
========================================
Learner: Alice (ID: 1)
Goal: Learn Python Basics
Skill Level: Beginner

Your Learning Path:
1. ✓ Variables (Start here!)
2. → Data Types
3. → Operators
4. → Conditionals
5. → Loops
6. → Lists
7. → Dictionaries
8. → Functions (Final goal)

Estimated completion: 4 weeks

✓ Study plan saved to outputs/study_plans/user_1_plan.txt
✓ Knowledge graph saved to outputs/knowledge_graph.png
```

---

## Testing

### Manual Testing Steps
1. Run `python main.py`
2. Create a new "Beginner" profile
3. Verify study plan starts from basics
4. Check that outputs/ folder contains generated files
5. Open knowledge_graph.png and verify it displays correctly

### Test Cases to Validate
- [ ] Profile creation with all skill levels works
- [ ] Knowledge graph loads without errors
- [ ] Study plan generation completes successfully
- [ ] Output files are created in correct locations
- [ ] Visualizations render properly

---

## Next Steps (Future Phases)

**Phase 2:** Add recommendation system for learning resources
**Phase 3:** Implement gamification and feedback features
**Phase 4:** Add reinforcement learning for dynamic adaptation
**Phase 5:** Conduct pilot studies and evaluation

---

## Troubleshooting

**Problem:** `ModuleNotFoundError: No module named 'networkx'`
**Solution:** Run `pip install networkx matplotlib`

**Problem:** `FileNotFoundError` when loading data
**Solution:** Ensure data/ folder exists and contains concepts.json

**Problem:** Graph visualization doesn't display
**Solution:** Check matplotlib backend, try adding `plt.show()` after `plt.savefig()`

---

## License
Educational use only - Phase 1 prototype

## Author
[Your Name] - AI-Driven Personalized Learning System Project
```

***

