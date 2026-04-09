# Terraform Questions 01

## Question 1
The terraform.tfstate file always matches your currently built infrastructure.

A. True  
B. False  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: B**

The terraform.tfstate file represents the last known state of the infrastructure. If changes are made outside Terraform, it may not match actual infrastructure.

</details>

---

## Question 2
One remote backend configuration always maps to a single remote workspace.

A. True  
B. False  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: B**

A remote backend can support multiple workspaces for environment isolation.

</details>

---

## Question 3
How is the Terraform remote backend different than other state backends such as S3, Consul, etc.?

A. It can execute Terraform runs remotely  
B. It doesn't show output locally  
C. It is only available to paying customers  
D. All of the above  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: A**

Remote backend (like Terraform Cloud) can execute Terraform runs remotely, unlike S3/Consul which only store state.

</details>

---

## Question 4
What is the workflow for deploying new infrastructure with Terraform?

A. terraform plan → changes → apply  
B. Write config → terraform show → apply  
C. terraform import → changes → apply  
D. Write config → init → plan → apply  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: D**

Standard workflow:
- Write configuration  
- terraform init  
- terraform plan  
- terraform apply  

</details>

---

## Question 5
A provider configuration block is required in every Terraform configuration.

A. True  
B. False  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: B**

Some built-in providers (like null_resource) do not require explicit provider configuration.

</details>

---

## Question 6
Which command would you use to rerun a local-exec provisioner?

A. terraform taint null_resource.run_script  
B. terraform apply -target  
C. terraform validate  
D. terraform plan  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: A**

`terraform taint` forces recreation of the resource, triggering the provisioner again.

</details>

---

## Question 7
Which provisioner runs commands on the created resource?

A. remote-exec  
B. null-exec  
C. local-exec  
D. file  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: A**

remote-exec runs commands on the remote resource using SSH/WinRM.

</details>

---

## Question 8
Which of the following is not true of Terraform providers?

A. Individuals can create providers  
B. Community can maintain providers  
C. HashiCorp maintains some providers  
D. Vendors can create providers  
E. None of the above  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: E**

All statements are true.

</details>

---

## Question 9
What command must be run first in a new Terraform directory?

A. terraform import  
B. terraform init  
C. terraform plan  
D. terraform workspace  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: B**

`terraform init` initializes providers and backend.

</details>

---

## Question 10
How to find a resource IP if no outputs are defined?

A. terraform output  
B. terraform_remote_state  
C. terraform state list + show  
D. destroy & apply  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: C**

Use state commands to inspect resource attributes.

</details>

---

## Question 11
Which is not a principle of Infrastructure as Code?

A. Versioned infrastructure  
B. Golden images  
C. Idempotence  
D. Self-describing  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: B**

Golden images are useful but not a core IaC principle.

</details>

---

## Question 12
Descriptions in variables/outputs are stored in state file.

A. True  
B. False  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: B**

Descriptions are not stored in the state file.

</details>

---

## Question 13
What is the provider for this resource?

A. vpc  
B. main  
C. aws  
D. test  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: C**

Provider is identified by prefix like `aws_*`.

</details>

---

## Question 14
If infrastructure is manually destroyed, what should you do?

A. terraform refresh  
B. Automatic  
C. Edit state manually  
D. terraform import  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: A**

Refresh updates state to match real infrastructure.

</details>

---

## Question 15
What is NOT processed in terraform refresh?

A. State file  
B. Configuration file  
C. Credentials  
D. Cloud provider  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: B**

Refresh updates state only, not configuration files.

</details>

---

## Question 16
What does Terraform Module Registry expose?

A. Required inputs  
B. Optional inputs  
C. Outputs  
D. All of the above  
E. None  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: D**

It exposes all module inputs and outputs.

</details>

---

## Question 17
Can you expose local values using output?

A. True  
B. False  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: A**

Locals can be exposed via outputs.

</details>

---

## Question 18
Should secrets be stored in the same repo as Terraform code?

A. True  
B. False  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: B**

Secrets should be stored securely (Vault, Secrets Manager, etc.).

</details>

---

## Question 19
Which is NOT a string function?

A. split  
B. join  
C. slice  
D. chomp  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: C**

slice is for lists, not strings.

</details>

---

## Question 20
To manage existing GCP VMs with Terraform (choose two):

A. Create new VMs  
B. terraform import  
C. Write Terraform config  
D. terraform import-gcp  

<details>
<summary>Show Answer</summary>

✅ **Correct Answers: B and C**

You must:
- Import existing resources  
- Write Terraform configuration  

</details>
