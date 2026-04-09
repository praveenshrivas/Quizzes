# Terraform Quiz – Day 4

## Question 41
What features does Terraform Cloud provide? (Choose two)

A. Deployment visualization  
B. Automatic backups  
C. Remote state storage  
D. Web UI  

<details>
<summary>Show Answer</summary>

✅ **Correct Answers: C and D**

Provides remote state storage and a web-based UI for managing runs and workspaces.

</details>

---

## Question 42
Where does local backend store state?

A. /tmp  
B. terraform file  
C. terraform.tfstate  
D. user terraform.state  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: C**

State is stored in terraform.tfstate in the working directory.

</details>

---

## Question 43
Which cannot be used to keep secrets out of config?

A. Provider  
B. Environment variables  
C. -var flag  
D. secure string  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: A**

Providers do not manage secrets.

</details>

---

## Question 44
Disadvantage of dynamic blocks?

A. Cannot loop  
B. Repeat blocks  
C. Hard to read  
D. Slower  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: C**

Dynamic blocks reduce readability.

</details>

---

## Question 45
Only the creator of a plan can apply it.

A. True  
B. False  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: B**

Anyone with access can apply the plan.

</details>

---

## Question 46
How to reference AMI ID from data source?

A. aws_ami.ubuntu  
B. data.aws_ami.ubuntu  
C. data.aws_ami.ubuntu.id  
D. aws_ami.ubuntu.id  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: C**

Use `data.<type>.<name>.<attribute>`.

</details>

---

## Question 47
Which meta-argument defines explicit dependency?

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: depends_on**

Ensures correct resource creation order.

</details>

---

## Question 48
How to delete newly created VM managed by Terraform?

A. Destroy from all 16 VMs  
B. terraform destroy  
C. Delete state file  
D. Delete manually  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: B**

Terraform only tracks the created VM, so destroy removes only that.

</details>

---

## Question 49
What is the resource name used for reference?

A. dev  
B. azurerm_resource_group  
C. azurerm  
D. test  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: D**

Resource reference format: `<type>.<name>` → name is "test".

</details>

---

## Question 50
TF_LOG=DEBUG logs to syslog.

A. True  
B. False  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: B**

Logs go to stderr, not syslog.

</details>

---

## Question 51
Where to define backend?

A. terraform block  
B. resource block  
C. provider block  
D. datasource block  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: A**

Backend is configured inside terraform block.

</details>

---

## Question 52
Terraform uses local provider names outside required_providers.

A. True  
B. False  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: A**

Local names are used in configs.

</details>

---

## Question 53
Command to list workspaces?

A. terraform workspace  
B. terraform workspace show  
C. terraform workspace list  
D. terraform show workspace  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: C**

Lists all workspaces.

</details>

---

## Question 54
Providers always installed from Internet.

A. True  
B. False  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: B**

Can use local mirrors.

</details>

---

## Question 55
Best practice to protect state secrets?

A. Blockchain  
B. SSL  
C. Remote backend  
D. Signed providers  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: C**

Use secure remote backends with encryption.

</details>

---

## Question 56
When does terraform apply reflect changes?

A. Immediately  
B. When provider completes  
C. After state update  
D. Based on refresh  
E. None  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: B**

Depends on provider execution time.

</details>

---

## Question 57
How to reference name of second instance?

A. element(...)  
B. aws_instance.web[1].name  
C. aws_instance.web[1]  
D. aws_instance.web[2].name  
E. aws_instance.web.*.name  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: B**

Index starts at 0 → second = [1].

</details>

---

## Question 58
Provider is NOT responsible for?

A. API interaction  
B. Multi-cloud provisioning  
C. Resources exposure  
D. Managing changes  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: B**

Providers are service-specific, not multi-cloud.

</details>

---

## Question 59
Provisioners can be added to any resource.

A. True  
B. False  

<details>
<summary>Show Answer</summary>

❌ **Correct Answer: B**

Not all resources support provisioners.

</details>

---

## Question 60
What does terraform refresh detect?

A. Code changes  
B. Empty state  
C. State drift  
D. Corrupt state  

<details>
<summary>Show Answer</summary>

✅ **Correct Answer: C**

Detects drift between state and real infrastructure.

</details>
