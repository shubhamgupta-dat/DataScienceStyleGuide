# Collaborating with `.ipynb Notebooks`

As studies grow in scale and complexity, it becomes really difficult to track down on the study progressed and what pitfalls did the user avoided. Same thing happened with the best of us, working on a `ipynb` Notebook in a collaborative environment can be very tricky. Users would work on similar environment and we would end up repeating same mistakes too. 

I am mentioning some development guidelines so that the users can collaborate without tiptoeing on each others work and progress. I hope to help readers to write reproducible research and with proper documentation and standards.

### 1. Ensure Same Virtual Environment

We often tend to work on different machines while working on same problem. With variety of libraries available in market. It's the responsibility of the developer to use same python version, same kernel and stable version of the dependent libraries, across various machines. This would ensure reproducibility.

| **DOs** |
| --- |
| Freeze pip to requirements.txt |
| Use same kernel to run and install binaries. |
| Mention issues with the library if it does not work in particular environment. |

| **DON'Ts** |
| --- |
| `requirements.txt` should also contain the version of library. It should not just be the name of binary. |
| Don’t keep artifactory username and password in `requirements.txt` |

### 2. Folder Structure

Keep all your notebooks in a designated directory space. Usually if you keep notebooks folder that should also be fine. You can also restructure the notebooks into sub-directories. Like for Exploratory Data Analysis you can have a different sub directory than for Feature Set Creation.

Also introduce a `README.md` in the notebooks folder so that reader/developer can understand the progress and the structure

| **DOs** |
| --- |
| Notebook folder should be mentioned in the README.md of the git repository. |
| Sub Directories Names should make sense. |

| **DON'Ts** |
| --- |
| Notebooks should not be kept in multiple folder where it has no role as part of module. |

### 3. Nomenclature

All files must follow a strict nomenclature which should be decided by the team. A suggestive format would be to follow the following format:

`YYYYMMDD_Task_Accomplished_Author_v1.ipynb`

and the example for the same would be

`20211003_Raw_Data_Extraction_Shubham_v2.ipynb`

The user must understand that the notebooks that they writing should be of good quality. Hence the nomenclature for these files should be standard.

| **DOs** |
| --- |
| Strictly following a guideline will help in proper organisation of notebook |
| Share the nomenclature strategy on the README.md present in notebook directory. |

| **DON'Ts** |
| --- |
| While duplicating notebooks please remove suffixes like ‘Copy1' or 'Backup2’ |
| Please do not change the Notebook if you are not the author of the notebook. |

### 4. Structuring the Notebook

A notebook is a journal of computational work flow that developer/researchers use to showcase the conclusion they arrived at. Hence it is necessary to showcase the work in an organised manner. 

A proper narration gives the audience an easy chance to arrive at conclusion. And one key benefit of using Jupyter Notebooks is being able to interleave explanatory text with code and results to create a computational narrative. Rather than only keep sporadic notes, use explanatory text to tell a compelling story that has a beginning that introduces the topic, a middle that describes your steps, and an end that interprets the results. Describe not just what you did but why you did it, how the steps are connected, and what it all means. It is okay for your story to change over time, especially as your analysis evolves, but be sure to start documenting your thoughts and process as early as possible. 

Make sure to document all your explorations, even (or perhaps especially) those that led to dead ends. These comments will help you remember what you did and why. You can always remove these comments later if turning your notebook into a pipeline.

Follow a standard structure to separate the working code, imports and functions from the actual flow. Also, subdivide the section which maintains the partition of work and conclusions. You can follow a standard as mentioned below:

- Giving a Broader Idea about the Notebook
- Functional Imports
- Constants to be used in Notebook like seed, file_paths, training constants, etc.
- Custom Functions Made Specifically for the notebook
- Subdividing the notebook into multiple parts with relevant code flow. Use descriptive markdown headers to organise your notebook into sections that can be used to easily navigate the notebook and add a table of contents. 

_Once you feel that notebook is completed please run it completely end to end and assert your results. Restarting your kernel and running all cells is also a good final test of results._

| **DOs** |
| --- |
| Separate your explanations, hypothesis and conclusions from the coding part of it. |
| Start with a broader task on your mind and mention certain assumptions that you have made. |
| Use Markdown Cells to write your content and explanatory text to showcase your understanding |
| Please keep hyperlinks for all the important documents/links you have used for development or support your argument. |
| Put low-level documentation in code comments. |

| **DON'Ts** |
| --- |
| Do not keep your explanation, hypothesis or conclusions in the code block. |
| Do not write extra functions if operations can be handled with simpler lambda functions. |
| Do not change constants during notebook run. |

### 5. Internal Module Dependency

All the configs and python modules kept for usage in notebook should used via any or both of the following method:

Module packaged in a `whl` package and installed with working kernel 

Module file should be present in a sub directory of notebook folder with path added to the `sys.path` at the top of the notebook before any imports are made from module.

| **DON'Ts** |
| --- |
| Do not add `__init__.py` file for the relative import. Notebooks can be tricky. |
| Do not forget to add dependency, for the custom modules, in the `requirements.txt` file. |

### 6. Version Control Strategy

To keep a track on the progress of work, team should create their branch just for their own changes. This limited change will ensure the branch to avoid merge conflicts. All these changes needs to be checked in after every significant changes. 

Sometimes too many cooks can help to swarm on a single problem and resolve it. It also becomes necessary to keep a track on what progress or decision was made if multiple developers have created different notebooks for the same task. This should be mentioned in the README.md file. For example, 3 members, P, Q and R are working on fine tuning a model. They have their own approaches but after cross review we found out that Q did the best optimization. Hence we would add a remark in the README.md :

```
## Model Tuning:
**Notebook Used**: 20211007_Model_Tuning_Q_v3.ipynb
#### Description:
The cross validation metric was consistent for the model. 
Hence we are suggesting to use this XYZ model with improved results of 75%.
#### Previous Accepted Version:
20211001_Model_Tuning_P_v1.ipynb: 
The model performed with the best baseline metric of 67%.
```

You will also observe that P did a good job in baselining the model for v1. And it was an accepted result. But because we have something better now, the previous work needs to be recognised and parked.

| **DOs** |
| --- |
| Pull Request would be a must after a significant milestone from all the available branches. |
| `README.md` should also be updated with the hypothesis and significant findings whenever we reach a milestone |
| Define Milestones for the Team. So every developer is on the same page. |

| **DON'Ts** |
| --- |
| Do not abandon `README.md` because that will showcase the progress of your work. |
| Adding some experimental notebooks should have a regex so that it can be kept in `.gitignore` |

### 7. Building Pipeline

Writing a notebook is like venturing in an unknown territory. The developer is finding a path to land somewhere near to their goal. But what happens once they reach their destination, they help others to navigate to that sweet path. Building pipeline is something similar. A well researched notebook would like a chaos to the reader, but a well organised research notebook can help reader to recreate that same path and build pipelines out of it.

Keeping a structured Notebook as mentioned in part 4 helps in organising work. Notebook is a playground and cells interactivity make them vulnerable to accidental overwriting or deletion of critical steps by the user. When in doubt just run the whole system from the top cell, you can also run after restarting the kernel. 

To allow partial execution of complex analyses, break long notebooks into smaller notebooks that focus on one or a few analysis steps. Even if you want to save your intermediate work, just save the data with correct naming convention. Once the notebook is complete you can partially copy the functions, constants and imports to build a pipeline from it. And I cannot press on this point enough, please run your pipeline end to end once you believe it's completed. A partial check can also be scripted using argparse or click library.

| **DOs** |
| --- |
| Ensure partial functionality. Small snippets of code can bring code robustness. |
| Document what your pipeline is supposed to do. |
| Write function docstrings once you complete the pipeline |

| **DON'Ts** |
| --- |
| Do not write monolith code. |
| Do not hardwire variables which can be replaced. Eg. Name of the file |
| Please avoid hot-fixes in the code. |
| The pipeline and notebook should be aligned in such a manner that new research notebook after each milestone should be picked up from the latest notebook. This will hold up the integrity. |

### 8. Publishing Results

Decided notebook should be published as a PDF/HTML page after every milestone. This ensures the readability and that it reaches a wider audience.

[*Reference*](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1007007)

 
