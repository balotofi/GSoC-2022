
# GSoC 2022 Work Product Report

Organization

[LibreCube](https://librecube.org/)

Contributor : 

[Husseinat Etti-Balogun](https://github.com/balotofi)

Mentors:

[Artur Scholz](https://librecube.org/people/artur-scholz/)

[Juan Luis Cano Rodriguez](https://github.com/astrojuanlu)		


## PREFACE & ACKNOWLEDGEMENTS

For four months from June to September 2022, I did an internship at LibreCube, an open-source space and earth exploration initiative. LibreCube’s core vision is to enable everyone to get involved in building systems for the exploration of near and remote locations using open-source hardware and software. This internship project is a part of an overarching LibreSim project that aims to simulate satellite usage in space.

I worked on a project to replicate the orbital components of a simulator. The [project repository can be found here](https://gitlab.com/librecube/prototypes/python-libresim). The main content of the project is to model standardized components and services to replicate satellite equipment (object motion) in space. This topic suits my major in mechanical engineering, my interest in the space sector and newfound interest in software development.

Through the assignment, I did not only gain a lot of knowledge but more importantly, I also had a great chance to sharpen my skills in a professional working environment. Not less important than the astrodynamics terms that I have learnt is the communication skills that I have been trained and practiced through giving presentations in the form of progress updates, and discussing with my mentors.

I am very appreciative of Artur Scholz and Juan Luis, my mentors at LibreCube. Juan gave me very on-time valuable 	instructions and gave me extensive guidance regarding many practical issues. My gratitude goes to Artur for his encouragements and instructions during my internship period. He gave me feedback on my work and encouraged progress presentations in which I could present my progress to other contributors and organisation members.
With their patience and openness, my mentors created an enjoyable working environment.

Throughout the internship, I have also learnt many things about the ESA center in Germany whose benefits are far beyond what I could learn in another project. 
Lastly, I would like to thank members of the Open Source Community Africa (OSCA) for introducing me to this great opportunity in which I have developed myself academically, professionally and socially.
 
### List of Figures

- Figure 1	Model of Initial Project Objectives
- Figure 2	The UML Diagram
- Figure 3	Accomplished Sections Diagram

### List of Abbreviations 

- SMP:	Simulation Modelling Platform	
- SDD:	Software Design Document	
- ESA:	European Space Agency	
- EGOS:	ESA Ground Operation System 	
- ECSS:	European Cooperation of Space Standardisation	
- ICD:	Interface Component Document	
- LEO:	Low Earth Orbit	
- API:	Application Programming Interface 	
- JPL:	Jet Propulsion Laboratory 	
- NGO:	Non- governmental organization	
- NASA:	National Aeronautics and Space Administration 	

 	
> The orbit module provides means to model orbital/trajectory movement of an object in the solar system and includes the calculation of planet positions, perturbation effects on the orbit. It computes the position and velocity of the object, taking into account the forces applied. It also defines a number of generic coordinate systems, including one for each planet and a local one for the object (spacecraft) to be modelled.
 – Project Abstract

# 1. Introduction

## 1.1 Problem statement and project objectives 

### 1.1.1 Problem Statement

CubeSats are a class of research spacecraft called nanosatellites.[4] They are used primarily by universities for research missions, typically in low Earth orbits (LEO).[1] CubeSat missions are not limited to LEOs but can be considered as a tool to improve knowledge of deep space or as support for future manned missions to other planets.[2] Efficient software for the design, analysis and simulation of aerospace systems is most-often-than-not, proprietary.

The LibreSim project was born with the objective of solving the high-cost problem caused by the lack of high-precision non-proprietary simulation software. The project aims to demonstrate a highly portable simulation and test platform that allows seamless transition of mission development artefacts to flight products.[3]

In the area of simulation in personal space system projects and academic research, there is a vast quality gap between the freely available and proprietary software; therefore, costliness is still a serious problem to be solved.

### 1.1.2 Project Objectives

The structure of the project was already padded out in the proposal submitted. Artur made it clear that it wasn’t necessary to finish everything that was proposed but to start with what was necessary initially. So it was decided to aim to do the coordinate system service, create the celestial body class to hold the planet properties, create planet instance classes that would inherit for the celestial body class, create an orbit propagator class that would output the satellite movement from one position to another within a given time frame, and if possible create a small body class to emulate moons, asteroids, comets, etc.

Figure 1 shows the intended working principle of the orbital module. The coloured boxes indicate those sections that were expected to be completed over the four-month duration of the summer internship. They are as follows;
-	Coordinate system
-	Celestial bodies
-	Orbit propagator
-	Small body
  
![Model of Initial Project Objectives](https://user-images.githubusercontent.com/100206676/195456237-4e3035d3-5c1a-4a85-83d1-2aeffc8e40da.png)

## 1.2 Organisation of this report.

The report is organized as follows. Chapter 2 will introduce technical descriptions about the related concepts such as the generic simulation platform standard guides, packages used like SciPy and AstroPy. Chapter 3 is about communications between my mentors and I, the work generated on a bi-weekly basis, and reflections on my functioning, the unexpected circumstances and the learning goals achieved during the internship. Finally, I give a conclusion on the internship and future work in Chapter 4 according to my initial goals.

This chapter describes the concept and architecture of ESA standards guides, as well as the motivation of using certain astronomy packages. An important part of the module which supports the rotational elements is the scientific computation library called SciPy which will also be explained within this chapter. This chapter takes text from the official documents of the European Cooperation for Space Standardisation and European Space Agency guides [5,6] and published documents of the Python package owners.

This chapter is organized as follows: Section 2.1 describes the generic simulation platform standards. Section 2.2 explains the use of the programming language Python briefly. Section 2.3 explains the concept of SkyField and the reasoning behind the preference of one astronomy package over the other. Section 2.4 talks about the computing package SciPy. Finally, Section 2.5 gives a description about AstroPy.

# 2. Technical descriptions

## 2.1 Description of platform standards

During the first phase of the project, I spent time reading the ESA documentation on how the generic platform was supposed to be put together. Documents like the ESA SMP Handbook, Software Design Document (SDD) etc. 

The ECSS Standards intended to be applied together for the management, engineering and product assurance in space projects and applications. The standard defined terms of what shall be accomplished, rather than terms of how to organize and perform the necessary work. [5]

The SMP2 Handbook provided a high-level view of SMP2 and introduced all its concepts together with their realisation in C++ so I had to map the definitions and examples to Python. As a handbook, this document was less formal than the other specification documents. [6]

ESA Ground Operations System SDD and ICD) pinpointed the exact schema required for the generic modelling of such a platform as what we intended to build. It was very helpful in terms of understanding exactly what components were needed. My hiccup was having to translate it into code because I didn’t fully understand how to read the UML diagrams and interpret the descriptions at that time.

## 2.2 Python 

The LibreSim project is written in pure python with use of additional astronomical packages to aid in complex calculations within the orbital module. The programming language is commonly used in the space sector and is something I was familiar with before beginning the project.

## 2.3 SkyField

SkyField is a Python astronomy package used to compare positions values from code. This library was used to calculate the positions of the planets within derived corresponding coordinate systems. After trying the more widely used astronomy library to calculate positions (AstroPy), it was found that the same could be done with SkyField using fewer lines of code. It was not as easy to do coordinate system transformations with this package though.

## 2.4. SciPy 

SciPy is a scientific computation library used for additional complex calculations. When calculating rotations through the transformation of the positions vectors into quaternions, this package came in very handy. It eased the complexities of the transformations and representations.

## 2.5 AstroPy

AstroPy is a python astronomy package used for the majority of the calculations. Because of my lack of familiarity with SkyField, and having already written code previously for the calculation of object rotations in the same coordinate systems derived in the Earth class, the code was reused. There is more variety in the coordinate frames, reference frames and capabilities of this library.

# 3. Activity Log

This chapter is the core of the report, where the work done and knowledge gained are introduced and expatiated upon. The goal of this chapter is to show the application of the platform simulation guidelines on the object models that have been defined in chapter 2 with support of the language specific packages.

The main contribution of this chapter and also of this report is to demonstrate the effort put into understanding the purpose of the project and using that knowledge to build the capability of the platform.

## 3.1 Biweekly activities

This section is about the experiences had over the duration of the project and includes background information on the internship, the lessons and skills learned.

It summarises the contribution period and covers critical areas of information. The logs cover the period of sixteen weeks of activities with progress made every two weeks which included meetings attended, research conducted, discussions with other contributors and software used to complete tasks.

### Month 1-2 

| WEEK  | ACTIVITY  |  
|---|---|
| WEEK 1- 2  | [Jun 13-25] - Because there was no assigned tasks or structured meeting times within the first two weeks, I spent that time creating the functions from scratch. I got the functions from the ESA SMP handbook which is a guide that we were using to structure the platform as discussed in Chapter 2. Initially, that was what I was doing - defining function and creating classes based off of those generic instructions. | 
| WEEK 2-4  | [Jun 27- Jul 9] - I had a meeting with Juan where we had a mini code review to make sure that I was doing the correct thing. We pivoted to the Orbit Propagator and he introduced me to PoliAstro (which we decided would be the best package to use for the propagation). I had trouble running it in VSCode due to a virtual environment installation issue. Artur’s feedback on my code was that I didn't need to implement the ICD word for word, that I just needed to do things that helped complete the defined tasks. He then assigned my first task to create basic coordinate systems classes so I first had to find out the different types of coordinate systems.| 
| WEEK 4-6  | [Jul 11-23] - I was unaware how to implement it from scratch so Artur created it to work with the simulators architecture and assigned me to calculate the position vector of Earth as seen in the Sun coordinates system and that logic was to go into the `do_update` method of the derived coordinate system class. I reached out to a fellow contributor Hrishit who was working on the electrical module, so that he might explain the mapping from the design documents to Python for me. The commits I made during this time were in my [forked repo](https://gitlab.com/balotofi/python-libresim/-/commits/develop) but I deleted the branch when the code was deemed unuseful. | 

### Month 2-3 

|  WEEK | ACTIVITY  |  
|---|---|
| WEEK 6-8  | [Jul 25- Aug 6] - I got the position vectors updating but was getting some negative values and doubted their correctness. Juan suggested using SciPy’s transform rotation method to calculate the rotations. I had to do some research on what a quaternion was (the method of representation of the rotations). SciPy also has a method called `as_quat` that represents matrices, vectors, or Euler angle values as quaternions. I later started getting quantity conversion issues when subtracting positions from transformed vectors. It was around this time Artur suggested commit directly into the LibreSim codebase and gave me access to do so because of the issue of keeping up to date with the latest commits from develop in my fork. | 
| WEEK 8-10  | [Aug 8-20] - I created a merge request because I had finally got rotations outputting values in a coordinate system and I wanted to do the same for other coordinate systems. The code was manually merged. The mid-term evaluations fell during this time. I had frequent hospital visits at the time and travelled cross-country for personal reasons. Made 3 merge requests during this time: [Draft: Coord sys method updates - do update method for earth fixed system](https://gitlab.com/librecube/prototypes/python-libresim/-/merge_requests/11), [Coord sys method updates](https://gitlab.com/librecube/prototypes/python-libresim/-/merge_requests/10), and [Draft: Position and Rotation outputs from do_update methods of EarthCentricEcliptical and EarthCentricEquatorial](https://gitlab.com/librecube/prototypes/python-libresim/-/merge_requests/9). Commits were made in [this](https://gitlab.com/librecube/prototypes/python-libresim/-/commits/orbital-coordsys?author=Husseinat%20Etti-Balogun) and [this](https://gitlab.com/librecube/prototypes/python-libresim/-/commits/orbital-celestbods?author=Husseinat%20Etti-Balogun) branch. | 
| WEEK 10-12  | [Aug 22- Sept 3] - Artur did some architecture restructuring because it was understood that the way we had implemented the coordinate system was incorrect. My code that computed positions was compared to values from another package (SkyField) and the results were different so I had to correct it. Late August was when I was finally able to write code that computed similar values to SkyField for the position vectors with the appropriate coordinate systems. Artur then asked me to create a UML diagram for the module as he thought it would be helpful in understanding the concept of the architecture a little bit better. Made [Draft: Orbital coordsys](https://gitlab.com/librecube/prototypes/python-libresim/-/merge_requests/14) merge request. Commits were made in [this](https://gitlab.com/librecube/prototypes/python-libresim/-/commits/orbital-coordsys?author=Husseinat%20Etti-Balogun) and [this](https://gitlab.com/librecube/prototypes/python-libresim/-/commits/orbital-celestbods?author=Husseinat%20Etti-Balogun) branch. |

### Month 4

|  WEEK | ACTIVITY  |  
|---|---|
| WEEK 12-14  | [Sept 5-17] - Since the coordinate system layout was now a service where coordinate systems could only be registered, I needed to replicate my previous code in individual planet files so that they would be the ones printing the positions and rotations. Again, I thought I had to build it from scratch so I created the celestial body IDs using an enumerations class before being reminded that the package AstroPy already had necessary planetary data. Commits were made in [this](https://gitlab.com/librecube/prototypes/python-libresim/-/commits/orbital-celestbods?author=Husseinat%20Etti-Balogun) and [this](https://gitlab.com/librecube/prototypes/python-libresim/-/commits/orbital-celestials?author=Husseinat%20Etti-Balogun) branch. | 
| WEEK 14-16  | [Sept 19- Oct 1] - I had a meeting with Artur where he sent me some links to code that was not part of the simulator but still within the LibreCube system that I could use to calculate the rotations. I got the position vectors running with a minor bug and then I fell ill and took an excuse to take time off to recuperate. I was unable to download JPL ephemerides files because of my location so they were sent them to me through WeTransfer. The week after, I got rotations running as well and created a merge request [Earth and Jupiter do update rotations](https://gitlab.com/librecube/prototypes/python-libresim/-/merge_requests/17) that was reviewed by Juan. | 

## 3.2 Research done 

A lot of the knowledge I acquired over the course of these 4 months was academic in terms of the fundamental subject matter and not actual coding logic. As stated in section 3.1 previously, I had to research a lot of concepts prior to being able to understand how it would translate to code. The following text illustrates the research I did and the knowledge I gained from it.

### 3.2.1 Ephemerides

Prior to contributing to this project, I didn’t know what ephemerides were. This project was heavily founded on planetary data gotten from the National Aeronautics and Space Administration (NASA) Jet Propulsion Laboratory (JPL) Application Programming Interface (API) that constituted the ephemeris files. At the beginning of the project, a question that Juan brought up was which ephemerides file we should use and I didn’t know that there were different types or the reasons for the differences. Over the course of this project I have used three ephemeris files. When using AstroPy I only had to state within the code to use the default JPL ephemerides file but when using SkyField, because the files could not be downloaded due to my location, Artur and Juan had to download the files `de421.bps` and `de440s.bps` and send to me so I could manually load from my local file path.

### 3.2.2 Issue reporting on GitHub 

I said in section 3.1 that I ran into issues with my VSCode identifying and running my Python interpreter. I had never experienced such before and it seemed to be due to the virtual environment I was using but I didn’t know how to fix it. After a session with Juan where we couldn’t find a solution, he suggested I file an issue on the VSCode GitHub repository (something I had never done before). Though the issue has not been totally resolved, I am now capable and comfortable with posting issues when I run into technical errors with an open source software on the appropriate repository.

### 3.2.3 Coordinate Systems

When researching which coordinate systems to initially model, I found that there were five main coordinate systems, being the horizontal, equatorial, ecliptic, galactic and supergalactic systems. Each coordinate system named after its choice of fundamental plane.

As I tried to figure out which coordinate system to use, I came across the term ‘reference frame’ which led me into another spiral of research that honestly left me more confused than when I started. After reading more about the difference on several separate occasions, it finally stuck.


### 3.2.4 Quaternions

Before I had gotten the program to output the position vectors, the rotations were stated to be vectors, after some restructuring early on, these values changed to quaternions – a word I’d never seen before -  and I found myself having to learn what they were before I could fully understand how output rotations in that representation.

I repeatedly watched the YouTube channel 3Blue1Brown’s half-hour and 5 minute videos that explained the concept of quaternions as a 4d number system in our 3d space. The visualisation tool on the website that Ben Eater created and they reference was also a major help in solidifying the concept for me.

Understanding the application of the system and reading the SciPy documentation on how the different methods transformed Euler angles, rotation vectors, matrices and other values into quaternions also helped greatly.

### 3.2.5 Rebasing 

Due to the nature of the structure of the branches in the organisations, repository, I had to learn how to rebase because there would often be time when commits would be made to the main develop branch that affected the structure of the files and I would have to make my commits on top of those.
The concept of rebasing was very confusing to me at first but after frequently having to check back to my trusted website, I got the hang of it. 

## 3.3 Overview of module with UML 

![libresim Class diagram](https://user-images.githubusercontent.com/100206676/195455495-16480f43-9ab9-4cce-a09e-9fefea322230.png)

The UML diagram above shows the current architecture of the system and how the objects are made to interact with each other. The basis of the orbital system is the coordinate systems and how each planet can dictate the coordinate system it is deriving from the coordinate system tree. In the `do_update` method of the planet is where the appropriate calculations were put to update the position and rotation of the planet (or object) within each derived coordinate system.

Having only used a UML diagram once for the rough schema of personal project, I was relatively new to the concept. I was given the task of creating a UML diagram for the project so that I may be able to understand it better when blocked.

A major roadblock I had was the overall architecture of the project. How the individual parts were supposed to be setup, and work together within the project?
Being able to conceptualise how things should fit together and think of the pseudocode was also a little trying. Creating the UML diagram helped me understand better.

## 3.4 Feedback from midterm evaluation and Reflections

In this section I reflect on the internship in terms of my attitude towards it and feedback from my first evaluation. Regarding my initial goals, I shortly discuss my experiences; if I have achieved my goal, whether I experienced difficulties and what I think I have to improve. 

### 3.4.1 Feedback from midterm evaluation

From my initial feedback I learned that I needed to improve certain skills and knowledge to work in a professional environment.

Although you learn and develop the necessary skills and knowledge while working in an organization, there are several things that I could improve already. I was not totally clear on what exactly I need to do in terms of writing code to reach my project goals. Therefore, during the project, I had some difficulties to determine tasks that I could carry out. 

In advance of my internship I talked with Artur about the project in which I could participate and my less-than-rudimentary knowledge in astronomy, however clear agreements on my activities were not made. A more assertive attitude from my side could have helped. To prevent uncertainties in future projects I will pay more attention to making clear agreements and back-up plans.

Other aspects to which I want to pay attention in general are: defining a clear action plan and determine what knowledge is suitable. Also in the internship I have seen that it is important to have your exact coding tasks clear, because it guides you in the process. 

### 3.4.2 Reflections

At the beginning I did not have any experience of working with a non- governmental organization (NGO). Trying to operate as a non-profit entity, I saw the importance of financial support and personal capacity. The dependence on extern institutions and the need to have a flexible but disciplined attitude. During the contribution stage I also experienced spontaneous fluctuations in the codebase. At first instance unannounced changes were annoying, but it forced me to be flexible and to see what other things I could do.

More than I had expected, I experienced communication difficulties. This was completely one-sided and not the fault of my mentors at all because they put so much effort into creating a welcoming space for me to ask anything. But I often needed multiple (more than three) explanations on the same thing to fully understand and didn’t want to burden them. To contribute more to projects and to progress faster, I want to learn to make a more confident impression and to express my ideas and opinions more certain.

In conclusion, the internship was a useful experience. I have find out what my strengths and weaknesses are; I gained new knowledge and skills and met many new people. I achieved many of my learning goals, however for some the conditions did not permit to achieve them as I wanted. I got insight into what working with an NGO is like.

# 4. Conclusion & Future Work

In conclusion, the internship was a useful experience. I have find out what my strengths and weaknesses are; I gained new knowledge and skills and met many new people. I achieved many of my learning goals, however for some the conditions did not permit to achieve them as I wanted. I got insight into what working with an NGO is like.

## 4.1 Conclusion

The main goal of the project were not fully fulfilled which I am disappointed in myself about that.

![Accomplished sections diagram](https://user-images.githubusercontent.com/100206676/195456064-98afca5c-6529-4a0f-87b7-e769a52d2073.png)

The diagram (Figure 3) above shows in coloured boxes, what was achieved during the time of my attachment to the organisation. If you can recall, the proposed sections to be worked on were the coordinate systems, celestial bodies, propagator, and small bodies.

There is room for a lot of improvement in terms of my coding capabilities and consistency. On the whole, this internship was a useful experience. I have gained new knowledge, skills and met new people. I achieved some of my project goals, however for some the conditions did not permit. I got insight into professional practice.

## 4.2 Future Work

Since I was not able to finish the intended aim of the project, I plan to continue with the organisation to try to reach the goals initially proposed over the next month. Then continue to contribute to the project in whatever capacity I can indefinitely.
 
# References

[1] List of CubeSats. (2022, September 03). Wikimedia Foundation.
      https://en.wikipedia.org/wiki/List_of_CubeSats.

[2] Chantal Cappelletti, Daniel Robson, Cubesat Handbook,
      Academic Press, 2021, Pages 53-65, ISBN 9780128178843,
      https://doi.org/10.1016/B978-0-12-817884-3.00002-3.
      (https://www.sciencedirect.com/science/article/pii/B9780128178843000023)

[3] Justin Morris et. Al. Simulation-To-Flight (STF-1): A Mission to Enable CubeSat
      Software-based Validation and Verification, January 2016
      DOI:10.2514/6.2016-1464
      Conference: 54th AIAA Aerospace Sciences Meeting, AIAA SciTech At: San Diego, CA
      Volume: AIAA 2016-1464[4]

[4] CubeSats Overview. (2018, 14 Feb). In NASA.
       https://www.nasa.gov/mission_pages/cubesats/overview

[5] European Cooperation for Space Standardisation, Space engineering, Simulation Modelling Platform, ECSS-E-ST-40-07C, 2 March 2020

[6] SMP 2.0 Handbook, European Space Agency Directorate of Operations And Infrastructure, EGOS-SIM-GEN-TN-0099, Issue 1 Revision 2,28 October 2005


> "Astronomy compels
 the soul to look upward, and leads us from this world to another."

 

