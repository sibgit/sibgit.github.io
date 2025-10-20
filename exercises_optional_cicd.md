# Optional module exercises: GitHub Actions and GitLab CI/CD

<br>

[[_TOC_]]

<br>

:fire:
In the exercise of this module, we aim to build a simple Python code check
pipeline. Since building automation pipelines is done differently between
GitHub and GitLab, this exercise comes in 2 flavors:
* **Exercise 5A** is about implementing the pipeline for a **GitHub** repo.
* **Exercise 5B** does the same, but for a **GitLab** hosted repository.

Select either exercise 5A or 5B, depending on the platform that interests
you most.

<br>
<br>

## Exercise 5A - Introduction to automation pipelines: GitHub Actions [45 min]
**Objective:** learn the basics of creating automation pipelines in **GitHub**.

### A) Create a repository on GitHub

1. Change into the `optional_exercise_5` directory. You will see that it
   already contains a git repository with a couple of commits.
2. Go to your GitHub account and create a new repository. You will work on it
   alone, so you can make it private.  
   **Note:** do not initialize this repository on GitHub, because we already
   have a local Git repo that we want to push to our new repo on GitHub.
3. Add the GitHub remote to your local repo, and push the content of your
   local repo to the remote.  
   Here is a reminder of how to do that:
    ```sh
    git remote add origin <URL of your remote>
    git push -u origin main
    ```

<br>

### B) Your first workflow
So far so good - but let's admit it: we stayed in our comfort zone!
But this will all change now, as we create our first **GitHub Actions workflow**.

This first workflow will be very simple and not anything useful. It's just a
test to see if we can get a workflow to be picked-up by GitHub and run.

1. At the root of the local repository,
   **create a hidden directory named `.github`**.
2. Inside the `.github` directory, create a `workflows` directory.
3. Inside the workflows directory, create a file named `test-workflow.yml` and
   add the following content to it:

    ```yaml
    name: test-workflow
    run-name: Test workflow
    on: push

    jobs:
      # Comments can be added to the file like this.
      print-hello-world:
        runs-on: ubuntu-latest
        steps:
        - name: Say hello...
          run: echo “Hello World”
    ```

   This is a very simple job - it basically does nothing outside of printing
   `Hello World` to the terminal. It should in principle never fail.

<br>

### C) Create a Python code check workflow
Now that you know how to run a workflow, let's up our game a little and create
an actually useful workflow.

As you might have noticed, our repository contains
[Python](https://www.python.org) code, so let's create a workflow that performs
the following 2 tasks:

* **Job 1 (format-check):** check the format of our python code, to make sure
  that our code follows the "best practice" guidelines.
* **Job 2 (syntax-check):** check the syntax of our python code, to make sure
  we have no error.

Let's do this one step at a time and add only the first job for now:
1. **Create a new workflow file** named `python-code-check.yml` (note: make
   sure to create it at the **correct location**).
   * The **name** of the workflow should be `python-code-check`.
   * The **run-name** of the workflow should be `Python code check`.
   * The workflow should **run on every push** to the repository.

2. **Add a first job** named **`format-check`** to the workflow.  
   Here is the code for this job:

    ```yaml
    format-check:
      runs-on: ubuntu-latest
      steps:
      - name: Checkout git repo
        uses: actions/checkout@v4
      - name: Install Python
        uses: actions/setup-python@v4
      - name: Install black
        run: |
          python -m pip install --upgrade pip
          pip install black
      - name: Run black
        run: black --check .
    ```

    As you can see, this job has 4 steps:
    * It starts by making a checkout of our repository to the VM (virtual
      machine) that runs the job.
    * Then it installs Python on the VM.
    * Then it installs *[Black](https://github.com/psf/black)*, a Python
      format checker.
    * Finally it runs *Black* on all python files it can find in our repo.
      How *Black* works is not important for this exercise, just know that it
      looks at all Python code files it can find in the repository and will
      return an error if it detects that one or more files do not follow
      standard python formatting.

    **Important:** make sure to indent all lines correctly when you write
    your GitHub workflow files.

3. **Commit** your new workflow **and push** to the remote.

4. Go to the **Actions tab** of your repo on GitHub and watch as your your
   workflow runs... and fails!

5. Yes, there is a format problem in the `simple_script.py` file.
   To fix the issue, open the file and change the line:

   ```py
   git_service=get_platform_name()
   ```

   To:

   ```py
   git_service = get_platform_name()
   ```

   This should fix the formatting issue that *Black* detected.

6. **Commit the change** to the file and push to the remote. Then go back to
   the **Actions tab** of your repo on GitHub: now you should see that the job
   succeeds :tada:.

<br>

### D) Add a second job to the Python code check workflow
Now that we got the first job working, let's add a second job to our workflow.
This second job should run a Python code syntax checker called
*[Pylint](https://github.com/pylint-dev/pylint)*.

The job should be named **syntax-check**, and, just like our first job in the
workflow, this job should have 4 steps:
* **Step 1:** checkout the repository to the VM.
* **Step 2:** Install Python.
* **Step 3:** Install *Pylint*. The shell commands to install *Pylint* are the
  following:
  ```sh
  python -m pip install --upgrade pip
  pip install pylint
  ```
* **Step 4:** Run *Pylint* using the following command:
  ```sh
  pylint $(git ls-files '*.py')
  ```

Add this second job to the `python-code-check.yml` workflow, then commit your
changes and push to the remote.

The Python code in the repo does not contain any syntax error, so your new
job **should succeed**.

<br>

### E) Add a job that depend on jobs 1 and 2
Let's continue improving our `python-code-check.yml` workflow by adding
another jobs to it:
* **Job 3 (unit-tests):** runs [unit-tests](https://en.wikipedia.org/wiki/Unit_testing)
  on our code. But don't worry, you will not need to write any Python code - we
  already wrote the unit tests for you!

Unlike the first 2 jobs of the workflow, **jobs 3 should run conditionally**:
Specifically, **Job 3** should only run if jobs 1 (**format-check**) *and*
job 2 (**syntax-check**) complete successfully.

Here are the instructions for job 3, where you should notice the following:
* The **`needs: `** keyword is used to indicate that the job depends on other
  jobs. In this case, `needs: [format-check, syntax-check]` means that this
  job will depend on both jobs 1 (**format-check**) *and* job 2
  (**syntax-check**). In other words, job 3 will only run if both job 1 and 2
  complete.
* For this job, we require a **specific Python version: `3.11`**. This is done
  by adding the instructions `with:` and `python-version: '3.11'` to the
  step `actions/setup-python@v4`.

  ```yaml
  # Run unit-tests for our code.
  unit-tests:
    needs: [format-check, syntax-check]
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - uses: actions/setup-python@v4
      with:
        python-version: '3.11'
    - name: Install pytest
      run: |
        python -m pip install --upgrade pip
        pip install pytest
    - name: Run pytest
      run: |
        pytest test_*.py
  ```

**Add the job 3** to the `python-code-check.yml` workflow, and check that
all jobs in the workflow run as expected (go to the GitHub **Action tab** to
check this). In principle this should work out-of-the-box.

**To test that the conditional execution** really works, let's try to make
Job 2 fail by introducing an error into the file `simple_script.py`:
1. **Open the file** `simple_script.py` with a text editor.
2. **Change line 4** of the script from `import os` to `# import os` (this
   changes the code line to a comment). This will create a error detected by
   *Pylint*.
3. **Commit the change** to `simple_script.py` and push it to the remote.
4. You should now see that **job 2 fails** and therefore
   **job 3 should no longer be run**.

<br>

### F) Add a last job to `python-code-check.yml`
Almost done: let's add a final job to our `python-code-check.yml` workflow:
* **Job 4 (test-run-script):** runs the entire `simply_script.py` script, to
  verify that it runs without error (this is sometimes referred to as an
  "integration test").

You task is to add this new job to the workflow, so that:
* The job is named **test-run-script**.
* The job should only run if job 3 completes successfully.
* The job should install python version `3.11`.
* As its last step, the job should run the command:
  `GIT_HOST_SERVICE="GitHub" ./simple_script.py`

When you are done, commit your changes to the workflow and push them to the
remote.

To verify that your Job 4 is running properly:
* Go to the GitHub Actions tab.
* Click on the latest run of the "Python code check" workflow, then make sure
  that all 4 jobs completed.
* Click on job **test-run-script** (job 4), and in the details of the step
  that runs the script, you should see the text `Greetings, fellow GitHub user.`
  displayed.



<br>
<br>
<br>

## Exercise 5B - Introduction to automation pipelines: GitLab CI/CD [45 min]
**Objective:** learn the basics of creating automation pipelines in **GitLab**.

<br>
<br>
<br>
