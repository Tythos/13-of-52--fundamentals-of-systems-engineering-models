We're going to continue focusing on key overall concepts in engineering. We're targeting a 2nd- or 3rd-semester engineer undergrad survey level of engineering content, so you may find *just a touch* of calculus helpful, but it's not required. Instead, I want to try and introduce as many interesting engineering concepts to readers and viewers across the field!

Today we will focus on four particular models from systems engineering that have incredibly useful applications to your thought process and the way you see the world. These are specific ideas that can be considered in both quantitative and qualitative ways. I've selected these four models based off of how useful I've found them across a wide variety of interesting problems and situations, both in engineering and in life.

## State Space Representation

In a state space representation, we are considering how a controls process can interact with the system it seeks to control. The first element of the state space representation model represents the "PLANT" or "universe"--basically, everything external to our controller.

![the plant or universe](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/1ewt3tm26n9o2vdblylm.png)

Within the universe of things outside our controller, there is some specific subset of states we want to control (more on that later). Specifically, there is some *desired state* and we want to take action to ensure this plant (like a robot arm, or factory furnace, or anything else) is doing what we want it do--that is, it is operating towards the desired state.

There is some subset of information that you (as the controller) can "perceive" about this plant. There is some subset of *measurements* that you can determine using specific systems called "SENSORS". Those sensors are systems of their own, with their own transforms and behaviors, and from those sensors you draw specific information about what you want to control. These are effectively your inputs into your controller.

![sensors](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/owklaz2kbcy4i5nbss35.png)

Next up is the "CONTROLLER". The information from the sensors goes into the controller, whether it is digital or analog (or anything else). The controller is responsible for making decisions about how the "desired state" is to be achieved, based on the input received from the sensors.

As an aside, sometimes the "state" is characterized by a variable `x` (often it is a vector of numbers). The controller is rarely able to perceive the state directly; rather, there is an output (`y`) from the sensors. And based on the controller's understanding of the plant from the output `y` (which is often "inverted" in some way to divine the underlying state `x`), combined with some "desired state" `x0` (or specifically the difference between the estimated state and the desired state, `dx`), the controller will issue some sort of command to another set of systems.

![the controller](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/o27fj9dvtgevwh24m7pa.png)

Those systems are called "ACTUATORS". Much like an actuator in a robot arm, these are systems that take some command signal to perform an action that influences, in some small but controlled (and well-characterized) manner, the state of (or input into) the universe (and/or plant).

![actuators](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ugpyed4n62sm0k5cuavv.png)

Often you'll see this "input" labeled `u`. One "form" in controls engineering you'll often see is a representation of system dynamics using the following equations:

```
dx/dt = A * x + B * u

y = C * x + D * u
```

We are making some assumptions here--for example, we assume the system is linear and time-invariant so we can represent `A`, `B`, `C`, and `D` as constant-value matrices. These assumptions will be different in other kinds of systems, like non-linear systems (in which case, `A` might be a function of `x`). There may also be other input signals--for example, forcing functions or noise from the world or other environmental factors. Regardless, just in this form we have a very powerful target for mathematical tools to solve for ideal solutions to our controller.

Another key observation is that the dynamics of the system (`x` as it changes over time, characterized by the first equation being a differential) are rarely the same as the input or controlled signal (`u`), and are rarely the same as the signal we can observe (`y`). But this model captures all of these phenomenon quite neatly.

There are also different dimensions to each of these signals! Your control authority may only affect a subset of the desired state. This leads to interesting implications about what is (and isn't) controllable. This also captures the solution to Kant's fallacies in his critiques of reason--he asserted that we cannot truly know because we do not directly perceive, and are therefore limited to the information (for example) our eyes provide to us. But, if we understand our eyes well enough, we can correct for the "sensor" behavior and determine a much more accurate understanding of the "universe" truth or state.

## Optimal Control Model

I had a song I sang to my kids when they were very young kids--"when you want to go from point A to point B, you take a taxiiii!"

![a to be](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gmvrr0qhqx31wb0l1pba.png)

When you want to go from point A, and find yourself at point B some time in the future, that transition takes place through a space (perhaps a state space). That transition is a *path* through that space. Effectively, this is a boundary value problem and we are solving for a path.

That path will have some sort of cost associated with it--usually a function of each point on the path that can then be integrated to determine the cost of the solution. That cost (whether its dollars or some other non-monetary value) can then be subject to a minimization process, using many interesting and powerful techniques in calculus. (Of course, this will only work for a scalar cost! Reducing multi-objective optimization problems to a scalar cost is a tremendous challenge in underconstrained problems like design.)

So, we have three challenges, regardless of the space or how quantitative we are:

1. Where are you? (What is point `A`?)

1. Where do you want to go? (What is point `B`?)

1. How will you get there? (What is the path? What will it cost? And how can the path be chosen to minimize that cost?)

![a to b along path with cost c](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/wu7kpookoijgmx1l6888.png)

This is even relevant to issues as qualitative issues like policy. Everyone always wants to talk about `B` (where they want to go). Utopia! But no one wants to figure out where they are (`A`) or how they got there. And very, very few people have the expertise or competence to characterize, predict, or solve for a system that successfully manages a transition from `A` to `B` with an acceptable cost.

## Standard Stream Model

The next process model comes from computer science. Let's say you wanted to write a basic computer program in C++, something along the lines of the classic "Hello, World!":

```c++
#include <iostream>

int main(int nArgs, char** vArgs) {
    std::cout << "Farewell, cruel world!" << std::end;
    return 0;
}
```

Then we could build and run it like so:

```sh
$ gcc main.cpp
$ .\a.out
Farewell, cruel world!
```

As simple as it is, this captures a number of interesting facts about the way a computer program, as a transform, is modeled:

1. There is no input here but we could theoretically pipe in inputs from some other program or file contents

1. We do have outputs that are being printed to what is called the "standard output stream".

1. We aren't processing them but you can see the "args" values that would let us consider different flags or options

1. We also have some 'dependencies" that we include (or import) to define how our own particular transform takes place

So, let's characterize this transform model There is some input, which we will label `STDIN` in homage to the standard input stream. This results in the generation of output, which we will label `STDOUT ` for similar reasons.

![stdin and stdout](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/i87ltub9fxlhnm5v9n1z.png)

But depending on what you want the program to do, there may be other options that characterize (for example) the output format or other settings. These represent the "configuration" of the process (like the knobs on a machine) that customize *HOW* the transform takes place.

We will also have dependencies that the transform leverages. These effectively are a decomposition of the top-level transform into a variety of other transforms.

And lastly, you may have noticed we didn't mention `STDERR`. Often a transform may have some other form of "meta-output" or log messages--a different pathway by which a transform generates information that isn't directly part of the STDOUT. This could be controlled by `syslog` levels, for example.

![config, dependencies, and stderr](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5sxwdhh33y5iiqinrd2c.png)

This process model is incredibly useful and applicable to many different kinds of transforms. For example, this can be used to characterize a specific station on a factory floor. Or perhaps this is a stage of automation in a digital engineering activity. Any process that transforms information can use this model to characterize what it is doing. (Not to mention any transform that is captured by software!)

## Systems Engineering V-Model

Our fourth model is a "v-model" that characterizes creative activities. This is specifically derived from systems engineering but is universally applicable to any human creative endeavor.

First, you have some form of design. External inputs (like customer desires or architect drawings) characterize some sort of design that goes into an engineering process. 

Second, that engineering process results in the development of a solution. This could be the formation of specific schematics, a manufactured product, a webpage, or some other software application. This is a creative process because the inputs are typically under-constraining the problem at hand, and therefore engineers require some combination of heuristics, experience, best-practice, and judgement, and iteration to achieve.

Finally, the internal development process hands of the result to a delivery to or with external stakeholders. Because this goes "down and back up", it is typically depicted in a "v" shape, and while we only described a simple three-step model here, there are many, *MANY* variations on this model that introduce considerably more complexity.

![design, develop, and deliver](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/y5bmysezo8k3mmspzy05.png)

What are some examples, you ask? Why, I think we can show you!

![the nightmare of jcids](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ksfsowhozjernd2t70av.png)

Welcome to the nightmare of the JCIDS. This characterizes the process by which the government engineers and produces products for defense. It's very complicated--but nearly every step is characterized by a different variation on the "v" process. It's relevant to every step of the process--from research, to requirements development, to development, to initial production, to full-rate production, to sustainment and retirement... 

This model is also relevant to software. If you look at any sort of issue model (whether it's on Github, or Gitlab, or scrum, or any other sort of process), the "issue " lifecycle will look like this "v". Let's consider the classic "Bugzilla" bug lifecycle model, for example:

![bugzilla lifecycle model](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8hjo4my53osb6rrrx3r7.png)

Stages 1-3 are "external"--a report or issue is generated (or designed), or maybe a new feature is proposed. In steps 3-5, the solution is developed by an engineer assigned to the task. Finally, in steps 5-7, some external verification and assessment (or acceptance) activities take place.

Even if you  don't care about engineering--say you are a sculpture. Your client asks for a statue that will go in their garden. They'll tell you how big it needs to be, what aesthetic it needs to suit, etc. That high-level design is then considered by you as you start iterating your block of marble. And hopefully, you'll deliver that statue to the client resulting in great satisfaction for everyone.

## Summary

Let's review our four important models:

1. The *state space representation* (or SSR) shows how a controller and related systems are put together to achieve some desired state of a controlled system, or "plant", through a combination of sensors, actuators, and controllers that operate on (or at least consider) some combination of input, state, and output signals.

1. The *optimal control model* defines how a system can transition through state space from point A to point B, using a path with an associated cost C that can be integrated and minimized. (An "optimal" control is one that achieves the globally-minimal cost, by the way! Bonus points.)

1. The *standard stream model* (or SSM) uses computer science to characterize a transform from input to output, as defined by specific configurations and dependencies, along with an optional "meta-output" or error stream for logs and monitoring.

1. The *systems engineering v-model" (or SEVM) shows the process by which external requirements and objectives, initially defining an under-constrained design problem, are subject to development in order for engineering to generate a solution that is delivered back to external stakeholders.

![the four horsemen of systems engineering models](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/kvuglygyv038hde8st0v.png)

These four models are incredibly useful, both in systems engineering and just in general thinking about life. They are also somewhat related--for example, when I talk about concepts like "state space" that are relevant to multiple processes. And once you "grok" these, you'll start noticing them everywhere in life! Many people know about one or two of these, but being able to put them all together turns your brain into an engineering superpower.
