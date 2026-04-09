# Terraform Quiz – Day 3

## Question 21
How would you solve inconsistent manual deployments using Infrastructure as Code?

A. Ticketing workflow  
B. Checklist  
C. Bigger instances  
D. Provisioning pipeline with version control  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: D**

Use a provisioning pipeline with version control and code reviews to ensure consistent deployments.

</details>

---

## Question 22
terraform init initializes a sample main.tf file.

A. True  
B. False  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: B**

terraform init initializes providers and backend, not configuration files.

</details>

---

## Question 23
Which two steps are required to provision infrastructure? (Choose two)

A. Destroy  
B. Apply  
C. Import  
D. Init  
E. Validate  

<details>
<summary>Show Answer</summary>

✅ **Correct Answers: B and D**

terraform init and terraform apply are essential steps.

</details>

---

## Question 24
Why use terraform taint?

A. Destroy resource  
B. Destroy and recreate resource  
C. Ignore resource  
D. Destroy all  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: B**

Forces resource recreation on next apply.

</details>

---

## Question 25
Terraform requires Go runtime installed.

A. True  
B. False  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: B**

Terraform is distributed as a compiled binary.

</details>

---

## Question 26
When should you use force-unlock?

A. Lock error  
B. High priority change  
C. Unlock failed automatically  
D. Apply failed  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: C**

Used when Terraform cannot automatically unlock state.

</details>

---

## Question 27
Which is NOT a valid module source?

A. FTP  
B. GitHub  
C. Local path  
D. Module Registry  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: A**

FTP is not supported.

</details>

---

## Question 28
Which feature is only in Terraform Cloud/Enterprise?

A. Secure variable storage  
B. Multi-cloud support  
C. terraform plan  
D. Workspace as data source  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: A**

Secure encrypted variable storage is a Cloud/Enterprise feature.

</details>

---

## Question 29
terraform validate checks syntax.

A. True  
B. False  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: A**

Validates syntax and configuration structure.

</details>

---

## Question 30
How to preview resources to be destroyed? (Choose two)

A. terraform plan -destroy  
B. Not possible  
C. terraform state rm  
D. terraform destroy  

<details>
<summary>Show Answer</summary>

✅ **Correct Answers: A and D**

Both show resources before destruction.

</details>

---

## Question 31
Correct way to pass variable to module?

A. servers = num_servers  
B. servers = variable.num_servers  
C. servers = var(num_servers)  
D. servers = var.num_servers  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: D**

Correct syntax is `var.<name>`.

</details>

---

## Question 32
Provisioner must be inside a resource block.

A. True  
B. False  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: A**

Provisioners are tied to resources.

</details>

---

## Question 33
Terraform requires Windows Server OS.

A. True  
B. False  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: B**

Terraform is cross-platform.

</details>

---

## Question 34
What does local backend store?

A. tfplan  
B. Binary  
C. Plugins  
D. State file  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: D**

Local backend stores terraform.tfstate.

</details>

---

## Question 35
How to format Terraform code properly?

A. terraform fmt  
B. Manual formatting  
C. Team member formatting  
D. Custom scripts  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: A**

terraform fmt ensures standard formatting.

</details>

---

## Question 36
What is advantage of private module registry?

A. Public sharing  
B. Version tagging  
C. Restricted access  
D. Public access  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: C**

Allows restricted access within organization.

</details>

---

## Question 37
What does terraform init NOT do?

A. Download providers  
B. Connect backend  
C. Download modules  
D. Validate variables  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: D**

Validation happens during plan/apply.

</details>

---

## Question 38
How to extract IDs from list of objects? (Choose two)

A. { for o in var.list : o => o.id }  
B. var.list[*].id  
C. [ var.list[*].id ]  
D. [ for o in var.list : o.id ]  

<details>
<summary>Show Answer</summary>

✅ **Correct Answers: B and D**

Both correctly extract IDs.

</details>

---

## Question 39
Which argument is required in variable block?

A. type  
B. default  
C. description  
D. All  
E. None  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: E**

None are mandatory.

</details>

---

## Question 40
How to specify module version from registry?

A. Not supported  
B. ?ref=v1.0.0  
C. version = "1.0.0"  
D. Default always 1.0.0  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: C**

Use version argument in module block.

</details>
