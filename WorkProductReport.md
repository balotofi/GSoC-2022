<div id="header" align="center">
<img src="https://user-images.githubusercontent.com/100206676/171044355-a1331115-6368-40f7-8061-3179192dca81.png"![logo_librecube_1604]() width="400"/>
</div>

<div id="header" align="center">
<img src="https://user-images.githubusercontent.com/100206676/171044297-58d64ef7-38e2-4d20-9a14-3c9d04083eb1.png"![gsoc]() width="500"/>
</div>

# Google Summer of Code 2022

## Contributor - Hussenat Etti-Balogun
[Gitlab Project link](https://gitlab.com/balotofi/python-libresim/-/tree/orbital)

## Generic Model - Orbital Abstract
> The orbit module provides means to model orbital/trajectory movement of an object in the solar system and includes the calculation of planet positions, perturbation effects on the orbit. It computes the position and velocity of the object, taking into account the forces applied. It also defines a number of generic coordinate systems, including one for each planet and a local one for the object (spacecraft) to be modelled.

## Core Tasks:
The following tasks were identified at the start of the project to be completed.

- [ ] Create celestial body objects - **injected from Astropy Library**
- [ ] Define local coordinate system - **completed**
- [ ] Define coordinate systems for planets - **completed**
- [ ] Model trajectory of an object {propagation service} - **not started**
- [ ] Define perturbation effects on orbit {position, velocity of object, forces applied} - **removed from scope of project**

## Technical Aspects

For developing the code, below mentioned technologies were used.

[Python](https://www.python.org/) - programming language that the whole project uses

[Skyfield](https://rhodesmill.org/skyfield/) - a python astronomy package used to compare positions values from code

[Astropy](https://www.astropy.org/) - python astronomy package used for the majority of the calculations

[SciPy](https://scipy.org/) - scientific computation library used for additional complex calculations

[Element](https://element.io/) - messaging platform where all communications were made


## Scope of Project
The UML diagram below shows the intended 
![libresim Class diagram](https://user-images.githubusercontent.com/100206676/194293900-2875a911-6933-42c5-85a5-b8d18a170e57.png)

## Reflections

During the first phase, we spent time reading the ESA documentation on how the generic platform was supposed to be put together. Documents like the SMP Handbook, Software Design Document (SDD) etc. After a general understanding was obtained, the task to create a basic coordinate system was issued. After initially starting with the propagator and discussing with both mentors, the package Astropy was chosen to be used to create the coordinate systems (as it had the frames, planets, calculations needed).

During the second phase, my mentor found that we had interpreted the initial oragnisation of the coordinate system part of the architecture wrong and and changed it accordingly. I moved on to creating the logic to callculate and output the planet positions within the three initial coordinate systems.

The major roadblock I had was the overall architecture of the project. How the individual parts were suppoed to be setup, and work together within the project. Being able to conceptualise how things should fit together and think of the pseudocode was also a little trying.


### Takeaways
Overall, it was a fun journey. I would thank my mentors Artur, Juanlu, and my fellow gsocer Hrishit, for guiding me throughout the project. I enjoyed learning a whole bunch of new things and writing useful codes.

### Merge Requests
The only lasting contributuion I had to the code base was manually merged and therefore has no merge request tag.



