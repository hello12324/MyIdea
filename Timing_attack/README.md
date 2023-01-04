[中文说明](./README_CN.md)
# illustrate
Timming Attacks (TA?), which is a password attack method that uses time-based timing to judge and count the time returned in the past to crack passwords or keys.
The principle is that if the program verifies the password. The code can be seen in the PassworldProgram subproject of my MyProgramProject project, which contains similar code.

```
input value => program => 'passworld' == input value => output
 
If the program is just a simple judgment, it may be due to the defect of the underlying function that the return time will be different when verifying the first character, such as the following chart
```
| input value |   password  | return time | return result |
|-------------|:------------|:-----------:|--------------:|
|'p'          |'passworld'  | 102ms       | Flase         |
|'a'          |'passworld'  | 93ms        | Flase         |
|'pa'         |'passworld'  | 101ms       | False         |
|By analogy, if the first character is the same as the first character of the password, it will |
|The time is different because of the calibration password.                                     |

# Repair method
Calculate whether the lengths of the two values are the same, if not, return False (0), if the lengths are the same, perform XOR operation, and return True (1) when the two values are the same, the code is implemented in the AntiTimmingAttack sub-project of the MyProgramProject project.

> It’s just a simple explanation, the real situation will be more complicated, but someone has actually cracked the ssl key certificate of apache, that is, David Brumley and Dan Boneh cracked it. You should see a pdf file in this directory, which is It's a pdf written by these two "geniuses", explaining how he cracked it.

> I will read this pdf file in detail later and briefly explain its detailed principles to make it easier to understand.
