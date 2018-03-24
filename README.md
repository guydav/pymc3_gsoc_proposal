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
[HDDM](https://github.com/hddm-devs/hddm) is a pymc2-based python library for hierarchical Bayesian parameter estimation the Drift Diffusion Model, a model used in cognitive neuroscience model to study decision making. After conversations with Michael J. Frank and Thomas Wiecki, we came up with the following project proposition, intended to migrate HDDM to pymc3 to support its continued development and extend its capabilities. As I envision the process, if I document it sufficiently thoroughly, my work could also serve as a guide for domain experts in other (not statistics) disciplines to implement custom likelihoods and toolboxes for recurring problems in their fields. 

### Pre-GSoC
I have been conversing with Michael J. Frank since February, aiming to contribute to his lab's work over the summer. I have familiarized myself with some of the theory around the model and filled gaps I had in my understanding of hierarchical Bayesian methods. After reading about a dozen papers on different relevant statistical methods, I began learning my way through the HDDM implementation and working with pymc3 (I have worked with Stan before, providing some background). Before GSoC begins, I will continue familiarizing myself with the details of HDDM's codebase, particularly the likelihood function (implemented in Cython). I would like to begin learning Theano, in preparation for porting the likelihood function. I will also get my feet wet with pymc3 development by attempting to tackle one or two of the issues tagged as 'beginner-friendly' and 'good-first-issue'. 

### GSoC
The first milestone to port HDDM to pymc3 would be to reimplement the likelihood function in Theano. HDDM uses a highly optimized version of the DDM likelihood function [(Navarro & Fuss, 2009)](http://psycnet.apa.org/record/2009-11068-003). Making the likelihood function Theano-friendly is the first step to any effort to migrate HDDM to pymc3. I will be able to compare results to the existing implementation as a form of regression testing. If done well, it should also allow computing gradients, allowing to use NUTS rather than the current (optimized) slice sampler. I hope to document this process well to facilitate practitioners from other fields to implement likelihood functions in Theano. 

Under the hood, HDDM currently uses [kabuki](https://github.com/hddm-devs/kabuki) to simplify the creation of hierarchical models. Thomas Wiecki suggested porting over the backend to [bambi](https://github.com/bambinos/bambi), a newer take on similar ideas. This step would require developing a better understanding of what kabuki currently does for HDDM and how Bambi fits in its stead. If done well, this should allow arriving at a working, pymc3-based implementation of HDDM. I would also like to document this process adequately. While the use case of switching from pymc2 to pymc3 is uncommon, the process of abstracting out some of the detail in lower levels (bambi and pymc3) and providing a gentler interface for practitioners is more generalizable. Again, should I document this sufficiently well, it might serve as a tutorial for creating toolkits above pymc3 and bambi. 

If I finish both of these milestones before the GSoC is over, I would next like to investigate alternative inference methods. While the DDM has an analytical likelihood function, many other recurring models in cognitive science do not. The next step I envision (and do not foresee myself finishing over the summer) is to implement some form of likelihood-free inference. I currently lean towards approximate Bayesian computation (ABC) methods, although some variational inference algorithms exist that might prove equally useful. I would (initially) focus my implementation on the types of problems that tend to arise in cognitive neuroscience, such as hierarchical model fitting and regression between neural correlates and parameters. 

### Post-GSoC

I intend to continue developing my work here as part of my senior thesis/project as part of my studies. Other than providing proper documentation and support, I foresee myself continuing to work on alternative inference methods in the upcoming fall and winter. Specifics will depend on the progress made during the summer and a better assessment of my capabilities and the project's priorities at the time.