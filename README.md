# Test Smell Examples

Provided below are examples of test smells that were detected in open source Android projects.

- [Assertion Roulette](#assertion-roulette)
#### Assertion Roulette

##### Source

App: [com.madgag.agit](https://github.com/rtyley/agit)

Test File: [GitAsyncTaskTest.java](https://github.com/rtyley/agit/blob/fc99d8eaa42940198589b032a2b9ba74d9ce3094/agit-integration-tests/src/main/java/com/madgag/agit/GitAsyncTaskTest.java)

Production File: [GitAsyncTask.java](https://github.com/rtyley/agit/blob/e42190e0f31f3d28086616f782e7f31422e9d229/agit/src/main/java/com/madgag/agit/operations/GitAsyncTask.java)

##### Code Snippet

```java
    @MediumTest
    public void testCloneNonBareRepoFromLocalTestServer() throws Exception {
        Clone cloneOp = new Clone(false, integrationGitServerURIFor("small-repo.early.git"), helper().newFolder());

        Repository repo = executeAndWaitFor(cloneOp);

        assertThat(repo, hasGitObject("ba1f63e4430bff267d112b1e8afc1d6294db0ccc"));

        File readmeFile = new File(repo.getWorkTree(), "README");
        assertThat(readmeFile, exists());
        assertThat(readmeFile, ofLength(12));
    }
```

##### Rationale

The `assertThat()` method is called 3 times within the test method. Each assert statement checks for a different condition, but the developer does not provide a explanation message for each assert statement. Hence, if one of the assert statements were to fail, identifying the cause of the failure is not straightforward. 

**[*[↑](#test-smell-examples)*]**
