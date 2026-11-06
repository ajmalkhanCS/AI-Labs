# Lab 1: Knowledge-Based Systems

This folder contains the files and implementation for Lab 1.

Knowledge-base and Inference Engine Lab + tracing forward chainmodify evaluate function

# Lab 01: Knowledge-Based Systems (KBS)

## Part 2: Hands-On Student Exercises

This section contains the additional code implemented to complete the two hands-on exercises assigned in the lab.

---

### Exercise 1: Degree Honors Rule

**Objective:** Add a new production rule to the existing Knowledge Base (`UNIVERSITY_KB`) for students who meet the degree honors requirements.

**Additional Code:**

```python
# Exercise 1: Degree Honors Rule

honors_rule = {
    "id": "RULE_07",
    "conditions": {"cgpa_min": 3.70, "disciplinary_warnings": 0},
    "condition_type": "degree_honors",
    "status": "Graduated with Magna Cum Laude Honors"
}

UNIVERSITY_KB["rules"].append(honors_rule)
```

**Code Explanation:**

1. `honors_rule` creates a new production rule using a Python dictionary.
2. `"id": "RULE_07"` assigns a unique identification number to the new rule.
3. `"cgpa_min": 3.70` specifies the minimum required CGPA.
4. `"disciplinary_warnings": 0` specifies that the student must have no disciplinary warnings.
5. `"condition_type": "degree_honors"` identifies the type of rule.
6. `"status"` defines the academic standing awarded to eligible students.
7. `UNIVERSITY_KB["rules"].append(honors_rule)` adds the new rule to the existing Knowledge Base.

**Result:** A new degree honors rule is successfully added to the Knowledge Base.

---

### Exercise 2: Tracing Forward Chaining

**Objective:** Modify the existing `evaluate_student()` function to display each evaluated rule condition during the inference process, demonstrating step-by-step logical deduction.

**Modified Code:**

Replace the original marks-range loop inside the `evaluate_student()` function with the following code:

```python
for rule in kb["rules"]:
    print(f"Evaluating {rule['id']}: {rule['conditions']}")

    if rule["condition_type"] == "marks_range":
        if rule["conditions"]["marks_min"] <= marks <= rule["conditions"]["marks_max"]:
            print(f"Rule {rule['id']} matched!")

            return {
                "rule_id": rule["id"],
                "grade": rule["grade"],
                "status": rule["status"],
                "action": rule["action"]
            }
```

**Code Explanation:**

1. `for rule in kb["rules"]` iterates through the rules stored in the Knowledge Base.
2. The `print()` statement displays the ID and conditions of each rule being evaluated.
3. `if rule["condition_type"] == "marks_range"` checks whether the current rule is a marks-range rule.
4. The nested `if` statement compares the student's marks with the minimum and maximum marks specified in the rule.
5. `print(f"Rule {rule['id']} matched!")` displays a message when a matching rule is found.
6. The `return` statement returns the matching rule's ID, grade, academic status, and recommended action.

**Example Output:**

```text
--- EVALUATING STUDENT: Ali Khan (ID: BSCS-2024-012) ---
Evaluating RULE_01: {'attendance_percentage': 74}
Evaluating RULE_02: {'marks_min': 85, 'marks_max': 100}
Rule RULE_02 matched!
Grade: A | Status: Passed with Distinction
Advisory Action: Recommended for Merit Scholarship.
```

**Result:** The inference engine now displays the rules being evaluated and identifies the matching rule during the deduction process.

---

### Conclusion

Both hands-on exercises have been implemented:

- **Exercise 1:** Extended the Knowledge Base by adding a new degree honors production rule.
- **Exercise 2:** Modified the Inference Engine to display rule evaluation and demonstrate step-by-step logical deduction.
