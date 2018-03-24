# Google Summer of Code Proposal: Migrating HDDM to pymc3

Also available on [Google Docs](https://docs.google.com/document/d/1nqHxB5HwLEuP2KWdGOKtGbjOh9Ocxi7EAXbCGSF0OCQ/edit?usp=sharing).

## Personal Information

My name is Guy Davidson. I'm a current student at the [Minerva Schools at KGI](https://minerva.kgi.edu), majoring in computational sciences with a focus on statistics and machine learning. Before enrolling at Minerva I spent six years in various software-related roles in the Israel Defense Forces' intelligence branch, and during my studies, I have interned at Amazon Prime Air and [Aidoc Medical](https://aidoc.com/). I'm fascinated by cognitive sciences and intend to pursue graduate studies in them. When I'm not around a computer, I love to read, play ultimate frisbee, and experiment with new cooking recipes.

I am applying to work on pymc3 as it aligns with both my interests and my background. I have software development experience in Python and highly enjoy writing clean and efficient code. My studies give me a reasonable starting point to understand the statistical methods underlying pymc3, and my interests in computational statistics and cognitive sciences match the project.

I will be available to work on the GSoC project full-time. I will be spending the summer at Michael J. Frank's lab at Brown, and this project aligns with the lab's goals. I am applying to attend a summer course at MIT starting August 9th, but I will make sure to find sufficient time during that last week of GSoC to tie up any loose ends. 

### Reference Projects
* **Minsketch**: [https://github.com/guydav/minsketch]() contains implementations of several count-min-sketch variants, initially written for a course final project, and then made available on PyPI. Fully documented here: http://minsketch.readthedocs.io/en/latest/
* **Coursework**: [https://github.com/guydav/minerva] contains most of the code I wrote for my classes at Minerva. The following might be particularly noteworthy:
    * The [CS152](https://github.com/guydav/minerva/tree/master/cs152) folder showcases implementations of an 8-Puzzle solver, an implementation fo the DPLL algorithm, and an implementation of [Counterfactual Regret Minimization](http://poker.cs.ualberta.ca/publications/NIPS07-cfr.pdf) for a final project. 
    * The [notebooks](https://github.com/guydav/minerva/tree/master/notebooks) folder contains Jupyter notebooks showcasing work with the standard toolkit and friends (`numpy, scipy, matplotlib, sklearn, Stan, ...`).

### Contact information
* **E-mail**: guy [at] minerva [dot] kgi [dot] edu
* **Skype**: guy_davidson
* **Phone**: +91-866-9622385
* **WhatsApp**: +972-545-777-495
* **GitHub**: [https://github.com/guydav/]()
* **LinkedIn**: [https://www.linkedin.com/in/guydav/]() 

## Project Plan
[HDDM](https://github.com/hddm-devs/hddm) is a pymc2-based Python library for hierarchical Bayesian parameter estimation the Drift Diffusion Model, a model used in cognitive neuroscience model to study decision making. After conversations with Michael J. Frank and Thomas Wiecki, we came up with the following project proposition, intended to migrate HDDM to pymc3 to support its continued development and extend its capabilities. As I envision the process, if I document it sufficiently thoroughly, my work could also serve as a guide for domain experts in other (not statistics) disciplines to implement custom likelihoods and toolboxes for recurring problems in their fields. I then intend to begin researching and implementing ABC methods as part of pymc3, with an eye towards methods useful in the sort of problems encountered in cognitive neuroscience, such as hierarchical model fitting and regression between neural correlates and parameters. 

### HDDM Background

HDDM is an open source Python package for using hierarchical Bayesian parameter estimation procedures (e.g., Gelman et al., 2013) to fit the Drift Diffusion Model (Ratcliff & McKoon, 2008) — a rigorous and highly influential process model of two-alternative forced-choice decision making — to behavioral and neural measures. It estimates the posterior probability of a trial-by-trial sequence of choices and response times. Hierarchical Bayesian parameter estimation optimizes the tradeoff between random and fixed effects models, capitalizing on the complete dataset by pooling information
from the group to inform estimation of individual subject parameters.

As noted by James Rowe (U Cambridge), "we have found that the HDDM gave accurate estimates of decision parameters with much fewer than 100 trials, in contrast to the hundreds or even thousands one might use for ‘traditional’ DDMs. This meant it was realistic to study patients who do not tolerate long testing sessions.” While other tools are available for fitting the DDM, HDDM provides a simple platform for users to fit hierarchical models that optimally trades off data derived from individual subjects and constraints imposed by statistical regularities across the group, improving parameter recovery relative to other methods, particularly in experimental designs for which the number of trials is not very large. It quantifies uncertainty in individual and group parameter estimates and provides simple measures to validate that a model can reproduce key features of the behavioral data via posterior predictive checks. A recent paper quantitatively compared HDDM to three other methods/packages (Ratcliff & Childers, 2016) and concluded that HDDM “performed very well, and is the method of choice when the number of observations is small.” 

The HDDM package has quickly gained popularity: the article describing it (Wiecki et al., 2013) has been cited over 166 times and, in 2017, the web documentation had an average of more than 1000 page visits per month. It enjoys a vibrant listserv (388 threads), has been adopted by a number of laboratories internationally, and is featured in more than 25 publications. HDDM thus serves as an excellent example of the value of a well-designed tool for automated parameter estimation, model validation and model comparison using posterior predictive checks. However, it is currently limited to the DDM, and even simple combinations of the DDM with other models and mechanisms is not yet possible within the software.


### Pre-GSoC
I have been conversing with Michael J. Frank since February, aiming to contribute to his lab's work over the summer. I have familiarized myself with some of the theory around the DDM and other similar models, and their relation to cognitive neuroscience. I filled gaps I had in my understanding of hierarchical Bayesian methods and approximate Bayesian inference. I read over a dozen papers on different approximate inference methods, both several classes of variational approaches, as well as different ABC methods. I began learning my way through the HDDM implementation and working with pymc3 (I have worked with Stan before, providing some background). 

Before GSoC begins, I will continue familiarizing myself with the details of HDDM's codebase, mainly the likelihood function (implemented in Cython). I would like to start learning Theano, in preparation for porting the likelihood function. I will also get my feet wet with pymc3 development by attempting to tackle one or two of the issues tagged as 'beginner-friendly' and 'good-first-issue.' 

### GSoC
The first milestone to port HDDM to pymc3 would be to reimplement the likelihood function in Theano. HDDM uses a highly optimized version of the DDM likelihood function [(Navarro & Fuss, 2009)](http://psycnet.apa.org/record/2009-11068-003). Making the likelihood function Theano-friendly is the first step to any effort to migrate HDDM to pymc3. I will be able to compare results to the existing implementation as a form of regression testing. If done well, it should also allow computing gradients, allowing to use NUTS rather than the current (optimized) slice sampler. I hope to document this process well to facilitate practitioners from other fields to implement likelihood functions in Theano. 

Under the hood, HDDM currently uses [Kabuki](https://github.com/hddm-devs/kabuki) to simplify the creation of hierarchical models. Kabuki is a general Python library (also developed by Wiecki, Sofer & Frank) for generating hierarchical pymc (not pymc3) models that are reusable, portable and more flexible. Kabuki models are classes with methods for maximizing the likelihood of the posterior estimate, saving and loading models, plotting output statistics, etc., and once created, can easily be applied to new datasets.

Thomas Wiecki suggested porting over the backend to [bambi](https://github.com/bambinos/bambi), a high-level Bayesian model-building interface using pymc3 as an inference backend. This step would require developing a better understanding of what kabuki currently does for HDDM and how Bambi fits in its stead. If done well, this should allow arriving at a working, pymc3-based implementation of HDDM. I would also like to document this process adequately. While the use case of switching from pymc2 to pymc3 is uncommon, the process of abstracting out some of the detail in lower levels (bambi and pymc3) and providing a gentler interface for practitioners is more generalizable. Again, should I document this sufficiently thoroughly, it might serve as a tutorial for creating toolkits above pymc3 and bambi. 

I will next begin implementing ABC methods in pymc3. MCMC methods rely on being able to compute the likelihood of the data under the model, which is not always possible. While the DDM features an analytical likelihood function, other relevant cognitive science models do not allow for an analytical solution, requiring reliance on the generative process or "implicit likelihoods." See, for instance, the Leaky Competing Accumulator (LCA; Usher & McClelland, 2001). ABC methods are among those best suited for parameter estimation in such models, as they operate by sampling potential parameters from model priors, simulating data, and comparing the simulated data to experimental or observed results. I aim to implement ABC methods that would be both useful generically, and particularly for the types of problems that tend to arise in cognitive neuroscience, such as hierarchical model fitting and regression between neural correlates and parameters. 

### Post-GSoC

I intend to continue developing my work here as part of my senior thesis/project as part of my studies. Other than providing documentation and support, I will continue integrating ABC methods into pymc3 in the upcoming fall and winter. After making sufficient progress on ABC methods, I would like to investigate likelihood-free variational approaches as well. Specifics will depend on the progress made during the summer and a better assessment of the project's priorities at the time.