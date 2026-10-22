 # Lab sheet 1: Answers        
 *Import, conflict, bisect, a leaked secret, a protected branch*



### Team Members:
- **Benrahmoune Israa** 
- **Boucherit Meriem**
- **Houdaci Lina Yacemine**
- **Mechraoui Aya**


--------


### Exercise 01:

- **(a)** `ls -a` shows two files whose names start with a dot. Which are: `.env.example` and `.gitignore.`
One is a template for settings and the other is a Git ignore file.



- **(b)**  Running the command `cp -r .../ctf-scoreboard/*` gave us a list of all the files except for the ones whose names start with a dot. `.gitignore` and `.env.example` are not in that list because `*` skips names that start with a dot. Unlike the result we got earlier, where both files were listed normally.



- **(c)** Adding `.env` to `.gitignore` does not protect it if the file was already committed because `.gitignore` does not remove files that Git is **already tracking**. The **previous commit** still contains the `.env` file, so its contents **remain in Git history** and can still be **recovered**.



--------
### Exercise 02:

- **(a)** Git cannot merge these two changes automatically because both branches changed the **same line** `(points: of the-long-game)` from the same original value `(500)` to **different values** `(450 and 600)`. Git cannot know which value we actually **want to keep**, so it creates a **conflict** and asks us to resolve it **manually**.


- **(b)**  If the two changes were made to two different challenges in the same file, Git could usually **merge them automatically** because the changes would not affect the **same lines**. There would be no **conflict** for Git to resolve.


- **(c)** The conflict-marker check can run automatically as part of a **CI/CD pipeline**. The pipeline can check the files for markers such as `<<<<<<< `and `>>>>>>>` and detect an **unresolved conflict** before the changes are **merged** into the **main branch**.

--------

### Exercise 03:

- **(a)** `git bisect run` needed **5 steps** in our case to find the first bad commit among the 31 commits.


- **(b)** `⌈log₂ 31⌉ = 5`, which is the same number of **steps** used by **git bisect**, and that is efficient because git bisect uses **binary search**.


- **(c)** If `check.sh` were inside the repository, its **availability** or **contents** could **change** when git bisect moves between different commits. For example, an **older commit** might **not contain** the script at all. This could make the test **unreliable**, so the script is placed **outside** the repository to keep the test **independent** from the commits being checked.


- **(d)** If the 31 commits were **squashed** into one commit, git bisect could no longer **identify** which of the original commits introduced the **bug**. It would only be able to identify the **single** remaining commit as the **bad commit**, since the **individual history** between the commits would no longer exist.

--------

### Exercise 04:

- **(a)** The exit code for the working-tree scan `gitleaks dir .` is **0**, while the exit code for the Git-history scan `gitleaks git -v .` is **1**.


- **(b)** The rule that matched the secret is **aws-access-token**.


- **(c)**  The second commit does not remove the secret from Git history because each commit keeps a **reference** to its **parent** commit. 
The first commit still **exists** in the history and still **contains** the secret, even though the second commit removed it from the **current version** of the file. Therefore, the secret can still be found when **scanning** the **Git history**.


- **(d)** If someone pushed a real key, the **first step** would be to **revoke** the exposed key so that it can no **longer be used**. The **second step** would be to **remove the secret** from the **repository** and **clean** the **history**.
 This order is important because **removing** the secret from Git does not **immediately** make an already-exposed key **invalid**, the key should be **disabled** first to reduce the **risk** of **unauthorized use**.

--------

### Exercise 05:

- **(a)** Branch protection is also a security control because it prevents **unauthorized** or **unreviewed changes** from being **pushed directly** to the **main branch**. Requiring a **pull request** and an approval adds a **review step** before changes become part of the main code. **Blocking force pushes** also helps protect the **existing commit history**.


- **(b)** The attack it makes harder is **code injection** through **unauthorized** or **malicious changes** to the main branch.


- **(c)** On a free GitHub account, the repository must be public for this part because the lab requires the **shared repository** to be public, and the free account's branch protection features used in this exercise require also a **public repository**.

