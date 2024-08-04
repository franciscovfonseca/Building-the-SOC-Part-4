<br>

<h1 align="center">Stage II - Building the SOC - Part 4</h1>

<br>

<h2 align="center">Tenant, Subscription & Resource Level Logging</h2>

![Banner](https://github.com/user-attachments/assets/39d59344-9d47-4be6-aba0-7ba2ead7efba)

<br />

<br />

<details close> 
<summary> <h2>1️⃣ Tenant Level Logging</h2> </summary>
<br>
  
Create **Diagnostic Settings** in Microsoft Entra ID.

This will allow us to Ingest Logs into our Log Analytics Workspace.

<br>

![azure portal](https://github.com/user-attachments/assets/6212a9be-938b-4627-a5f9-c5a50891b90f)

<br>

We'll create a **New User**:

- We can Name it ```Lain```
- Copy and Save the **Auto-generated Password**
- Click **"Review + create"** to Create the New User:

<br>

![azure portal](https://github.com/user-attachments/assets/6212a9be-938b-4627-a5f9-c5a50891b90f)

<br>

Assign the **Global Administrator Role** to the New User.

>   <details close> 
>   
> **<summary> 💡 Note</summary>**
> 
> We can go back to our **Log Analytics Workspace** to confirm that **Logs are Properly Being Ingested**.
> 
>   </details>

<br>

![azure portal](https://github.com/user-attachments/assets/6212a9be-938b-4627-a5f9-c5a50891b90f)

<br>

<h2></h2>

<br>

We'll now create another **New User** ➜ this is going to be our **"Attacker" User** ```Madara```

➡️ Generate some **Failed Authentication Logs** ➜ by failing to Log In 10 times in a row.

<br>

![azure portal](https://github.com/user-attachments/assets/6212a9be-938b-4627-a5f9-c5a50891b90f)

<br>

>   <details close> 
>   
> **<summary> 💡 Note</summary>**
> 
> The **AuditLogs** come in pretty quickly, but the **SigninLogs** and **AzureActivity** take a while to come into the **Log Analytics Workspace**.
>   
> Generate the logs, then take a 20-30 minute coffee break and query the LAW after.
> 
>   </details>

<br>

### Audit Logs:

<br>

![azure portal](https://github.com/user-attachments/assets/6212a9be-938b-4627-a5f9-c5a50891b90f)

<br>

### Signin Logs:

<br>

![azure portal](https://github.com/user-attachments/assets/6212a9be-938b-4627-a5f9-c5a50891b90f)

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2>2️⃣ Subscription Level Logging</h2> </summary>
<br>

We're now going to export the **Azure Activity Logs** to our **Log Analytics Workspace**.

Go to **Azure Monitor** ➜ then the **Activity Log** blade ➜ and click on ⚙️ **Export Activity Logs**

<br>

![azure portal](https://github.com/user-attachments/assets/6212a9be-938b-4627-a5f9-c5a50891b90f)

<br>

We'll then create a new **Diagnostic Setting**:

<br>

![azure portal](https://github.com/user-attachments/assets/6212a9be-938b-4627-a5f9-c5a50891b90f)

<br>

Now we'll **Generate some Logs** to confirm functionality.

To do so I decided to:

- Create 2 New **Resource Groups**
- Add a new **Inbound Security Rule** to 1 of the existing NSGs

<br>

### Signin Logs:

<br>

![azure portal](https://github.com/user-attachments/assets/6212a9be-938b-4627-a5f9-c5a50891b90f)

<br>

### New Inbound Security Rule in the ```attack-vm-nsg```:

<br>

![azure portal](https://github.com/user-attachments/assets/6212a9be-938b-4627-a5f9-c5a50891b90f)

<br>

💡 I then Deleted all these new Resources just to confirm the **Logs were Flowing into the LAW Properly**.

Back to our **Log Analytics Workspace** ➜ I **Queried the Logs** for any **Changes to the NSGs**:

<br>

![azure portal](https://github.com/user-attachments/assets/6212a9be-938b-4627-a5f9-c5a50891b90f)

<br>

I also performed another **Query for Checking Resource Group Deletion** in our Logs:

<br>

![azure portal](https://github.com/user-attachments/assets/6212a9be-938b-4627-a5f9-c5a50891b90f)

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2>3️⃣ Resource Level Logging (Data Plane Logs)</h2> </summary>
<br>

The next step is to Enable Logs for our Storage account and for our Key Vault.

<br>

### Storage Account:

<br>

Go to **Storage Accounts** ➜ and select our ```sacyberlab009``` Storage Account

On the left side under **Monitoring** ➜ select the **Diagnostic settings** blade

For the **"blob"** line ➜ click on ⛔ **Disabled** under **"Diagnostic status"** for the 

<br>

![azure portal](https://github.com/user-attachments/assets/6212a9be-938b-4627-a5f9-c5a50891b90f)

<br>

Click on ➕ **Add Diagnostic setting**

<br>

![azure portal](https://github.com/user-attachments/assets/6212a9be-938b-4627-a5f9-c5a50891b90f)

<br>

- Create a **"Diagnostic setting name"** ➜ I picked ```ds-storage-acct```

- Under **Logs** ➜ **Category groups** ➜ select ☑️ **audit**

- Make sure we're sending the Logs to our **Log Analytics Workspace** ```LAW-Cyber-Lab```

Click **Save**

<br>

![azure portal](https://github.com/user-attachments/assets/6212a9be-938b-4627-a5f9-c5a50891b90f)

<br>

<h2></h2>

<br>

### Azure Key Vault:

<br>

Navigate to **Azure Key Vault** ➜ click on **"Create a Key Vault"**.

⚠️ Make sure it is in the same Resource Group & Region as our other Resources.

Also ➜ the **"Key vault name"** must be Globally Unique.

<br>

![azure portal](https://github.com/user-attachments/assets/6212a9be-938b-4627-a5f9-c5a50891b90f)

<br>

Once the Key Vault is configured properly ➜ click on **"Review + Create"**.

<br>

![azure portal](https://github.com/user-attachments/assets/6212a9be-938b-4627-a5f9-c5a50891b90f)

<br>

We'll the create a **Diagnostic Setting** for the **Key Vaul**t the same way we did for the **Storage Account**.

<br>

![azure portal](https://github.com/user-attachments/assets/6212a9be-938b-4627-a5f9-c5a50891b90f)

<br>

- Pick a **"Diagnostic setting name"** ➜ ```ds-akv```

- Under **Logs** ➜ **Category groups** ➜ select ☑️ **audit**

- Once again ➜ make sure we're sending the Logs to our **Log Analytics Workspace** ```LAW-Cyber-Lab```

Click **Save**

<br>

![azure portal](https://github.com/user-attachments/assets/6212a9be-938b-4627-a5f9-c5a50891b90f)

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2>4️⃣ Generate some Logs</h2> </summary>
<br>

Finally, we're going to generate some logs by creating a Few Secrets inside of our Azure Key Vault

We're then going to **Query those Logs** and analyse them inside of our LAW.

<br>

### Secret Number 1:

<br>

![azure portal](https://github.com/user-attachments/assets/6212a9be-938b-4627-a5f9-c5a50891b90f)

<br>

### Secret Number 2:

<br>

![azure portal](https://github.com/user-attachments/assets/6212a9be-938b-4627-a5f9-c5a50891b90f)

<br>

Inside of our ```ak-cyber-lab``` **Key Vault** ➜ we can confirm that both Secrets were Created ✅

<br>

![azure portal](https://github.com/user-attachments/assets/6212a9be-938b-4627-a5f9-c5a50891b90f)

<br>

So we'll then Query our ```LAW-Cyber-Lab``` ➜ to make sure **Logs are Flowing from our Resources**:

<br>

![azure portal](https://github.com/user-attachments/assets/6212a9be-938b-4627-a5f9-c5a50891b90f)

<br>

  </details>

<h2></h2>

<br>

<br>

<br>

<br>

<br>

<br>

<br>
  
<br>
