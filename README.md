# cpp_CI_template

This template provides a starting point to learn how to set up CI in a C++ project that use CMake. If you don't know nothing about GitHub Actions and CI, see our [article](https://coollibs.github.io/contribute/Programming/creating-a-new-library) that explain what GitHub Actions is and what is it used for. You can find a code example [here](https://github.com/CoolLibs/cpp-CI-template/blob/main/.github/workflows/build_and_run_tests.yml).

## Set-up a workflow

First of all, you need to create a ```.github/workflows directory``` and add a .yml file on it.

### First (need to find a name)

First, you need to give a name to your workflow and choose the [trigger](https://docs.github.com/en/actions/using-workflows/triggering-a-workflow) of it. Then, you have to choose your target and this should be the name of the target you created in your CMakeLists.txt and want to build and run.

#### Example 
```yml
name: Build and run tests

on: 
  push:
    branches: [ main ]

  pull_request:
    branches: [ main ]


env:
  TARGET: cpp_CI_template-tests
  ```
Our workflows name is ```Build and run tests```, it's triggered on ```push``` and on ```pull_request``` on main and our target is ```cpp_CI_template-tests```

### Set up a job

Once you have your trigger and your target, you have to set up a job. In this example we have a job for each triplet (OS, compiler, build type) we needed. When setting up a job, you can give it a name and you have to give it the OS on which the job will be effective. There are several way to do it. As we have at least a job for each OS we needed we use 
``` yml
Windows_Clang_Debug:
    name: Windows Clang Debug
    runs-on: windows-2022 
```
To run on windows. You can find other runners and know more about it [here](https://docs.github.com/en/actions/using-jobs/choosing-the-runner-for-a-job). <br><br>
Once you have set it up, you can start giving steps to follow to your job. The first one is 
```yml
- uses: actions/checkout@v3
      with:
          submodules: recursive
```
This action checks-out your repository and his submodules under ```$GITHUB_WORKSPACE```, so your workflow can access it.<br><br>
You can now use different steps to configure your Cmake file and run your tests
#### Example

```yml
steps:
    - uses: actions/checkout@v3
      with:
          submodules: recursive

    - name: Configure CMake
      run: cmake .\tests -B ${{github.workspace}}\build -T ClangCL

    - name: Build
      run: cmake --build ${{github.workspace}}\build --config Debug --target ${{env.TARGET}}

    - name: Run
      run: ${{github.workspace}}\build\Debug\${{env.TARGET}}
```

## Results


Once your CI is set up, you can find the result on the ```Actions``` section of your repo.<br> <br> 
<img width="916" alt="Screenshot 2022-08-12 at 11 20 47" src="https://user-images.githubusercontent.com/44179059/184326097-4c07fae0-6ad6-4b1d-950a-fe306a8d6da7.png"> <br><br>

Then you can see all of your triggered workflows, there names and if they succeed or fail.<br><br> 
<img width="1227" alt="Screenshot 2022-08-12 at 11 23 12" src="https://user-images.githubusercontent.com/44179059/184326143-6d2919f0-a8f9-4dfa-8e3a-15e88e5a84c1.png"> <br><br>
If you clic on one you can also see all of the running jobs and how they're going.<br><br> 
<img width="1037" alt="Screenshot 2022-08-12 at 11 23 28" src="https://user-images.githubusercontent.com/44179059/184326237-eb49983a-f584-4ebc-9e73-9388c93c9527.png"><br><br>


## Ressources

You can find more about implementing a CI to your project with GitHub Action on the [official documentation](https://docs.github.com/en/actions/quickstart)
