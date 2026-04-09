# Terraform Quiz – Day 5

## Question 61
Which flag saves the execution plan to a file?

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: -out**

Used with `terraform plan -out=plan.tfplan` to save the plan.

</details>

---

## Question 62
What is the default Terraform state file name?

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: terraform.tfstate**

Stores infrastructure state.

</details>

---

## Question 63
A Terraform local value can reference another local value.

A. True  
B. False  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: A**

Locals can reference each other (no circular dependency).

</details>

---

## Question 64
Which is NOT a Terraform collection type?

A. list  
B. map  
C. tree  
D. set  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: C**

tree is not a valid type.

</details>

---

## Question 65
terraform taint immediately recreates resource.

A. True  
B. False  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: B**

It marks for recreation; happens during apply.

</details>

---

## Question 66
All backends support state storage, locking, and remote execution.

A. True  
B. False  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: B**

Not all support locking or remote execution.

</details>

---

## Question 67
How does terraform plan help?

A. Validates execution plan  
B. Initializes directory  
C. Formats code  
D. Updates state  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: A**

Shows changes without modifying infrastructure.

</details>

---

## Question 68
How to manage multiple environments with same config?

A. terraform import  
B. terraform workspace  
C. terraform state  
D. terraform init  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: B**

Workspaces allow multiple state files.

</details>

---

## Question 69
What is the resource name used for reference?

A. compute_instance  
B. main  
C. google  
D. teat  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: B**

Format: `<type>.<name>` → name is "main".

</details>

---

## Question 70
How to safely inject sensitive variables in CI/CD?

A. -var flag  
B. Store in code  
C. secure_vars.tf  
D. Plain text repo  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: A**

Pass at runtime using -var or environment variables.

</details>

---

## Question 71
How to protect secrets in state file?

A. Delete state  
B. Encrypted backend  
C. Edit state  
D. tfvars file  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: B**

Use encrypted remote backend.

</details>

---

## Question 72
Terraform Cloud workspaces act like separate directories.

A. True  
B. False  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: A**

Each workspace has isolated state.

</details>

---

## Question 73
How to enable detailed debug logs?

A. TF_LOG=TRACE  
B. Provider config  
C. TF_VAR_log  
D. TF_LOG_PATH  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: A**

TRACE gives maximum logging detail.

</details>

---

## Question 74
How is terraform import executed?

A. init  
B. plan  
C. refresh  
D. explicit command  
E. all  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: D**

Must be run manually.

</details>

---

## Question 75
If VM deleted manually, what happens on apply?

A. Removed from state  
B. Error  
C. No change  
D. Recreated  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: D**

Terraform recreates missing resource.

</details>

---

## Question 76
Most secure way to store backend secrets?

A. Environment variables  
B. Backend block  
C. External config  
D. None  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: A**

Avoid hardcoding credentials.

</details>

---

## Question 77
Which backend will NOT work?

A. S3  
B. Artifactory  
C. Git  
D. Terraform Cloud  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: C**

Git is not a Terraform backend.

</details>

---

## Question 78
Default backend in Terraform?

A. Terraform Cloud  
B. Consul  
C. Remote  
D. Local  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: D**

Local backend is default.

</details>

---

## Question 79
Where are modules cached?

A. /tmp  
B. Memory  
C. .terraform directory  
D. Not cached  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: C**

Stored in `.terraform` folder.

</details>

---

## Question 80
Why will terraform apply fail initially?

A. Formatting required  
B. Plugins not installed  
C. Login required  
D. Plan required  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: B**

Run terraform init first to install providers.

</details>
