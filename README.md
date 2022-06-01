# directoryBestPractice

Package by feature, not layer

The first question in building an application is "How do I divide it up into packages?". For typical business applications, there seems to be two ways of answering this question.
Package By Feature
Package-by-feature uses packages to reflect the feature set. It tries to place all items related to a single feature (and only that feature) into a single directory/package. This results in packages with high cohesion and high modularity, and with minimal coupling between packages. Items that work closely together are placed next to each other. They aren't spread out all over the application. It's also interesting to note that, in some cases, deleting a feature can reduce to a single operation - deleting a directory. (Deletion operations might be thought of as a good test for maximum modularity: an item has maximum modularity only if it can be deleted in a single operation.)
In package-by-feature, the package names correspond to important, high-level aspects of the problem domain. For example, a drug prescription application might have these packages:

com.app.doctor
com.app.drug
com.app.patient
com.app.presription
com.app.report
com.app.security
com.app.webmaster
com.app.util
and so on...
Each package usually contains only the items related to that particular feature, and no other feature. For example, the com.app.doctor package might contain these items:
DoctorAction.java - an action or controller object
Doctor.java - a Model Object
DoctorDAO.java - Data Access Object
database items (SQL statements)
user interface items (perhaps a JSP, in the case of a web app)
It's important to note that a package can contain not just Java code, but other files as well. Indeed, in order for package-by-feature to really work as desired, all items related to a given feature - from user interface, to Java code, to database items - must be placed in a single directory dedicated to that feature (and only that feature).

In some cases, a feature/package will not be used by any other feature in the application. If that's the case, it may be removed simply by deleting the directory. If it is indeed used by some other feature, then its removal will not be as simple as a single delete operation.

That is, the package-by-feature idea does not imply that one package can never use items belonging to other packages. Rather, package-by-feature aggressively prefers package-private as the default scope, and only increases the scope of an item to public only when needed.

Package By Layer
The competing package-by-layer style is different. In package-by-layer, the highest level packages reflect the various application "layers", instead of features, as in:
com.app.action
com.app.model
com.app.dao
com.app.util
Here, each feature has its implementation spread out over multiple directories, over what might be loosely called "implementation categories". Each directory contains items that usually aren't closely related to each other. This results in packages with low cohesion and low modularity, with high coupling between packages. As a result, editing a feature involves editing files across different directories. In addition, deleting a feature can almost never be performed in a single operation.
Recommendation: Use Package By Feature
For typical business applications, the package-by-feature style seems to be the superior of the two:
Higher Modularity
As mentioned above, only package-by-feature has packages with high cohesion, high modularity, and low coupling between packages.

Easier Code Navigation
Maintenance programmers need to do a lot less searching for items, since all items needed for a given task are usually in the same directory. Some tools that encourage package-by-layer use package naming conventions to ease the problem of tedious code navigation. However, package-by-feature transcends the need for such conventions in the first place, by greatly reducing the need to navigate between directories.

Higher Level of Abstraction
Staying at a high level of abstraction is one of programming's guiding principles of lasting value. It makes it easier to think about a problem, and emphasizes fundamental services over implementation details. As a direct benefit of being at a high level of abstraction, the application becomes more self-documenting: the overall size of the application is communicated by the number of packages, and the basic features are communicated by the package names. The fundamental flaw with package-by-layer style, on the other hand, is that it puts implementation details ahead of high level abstractions - which is backwards.

Separates Both Features and Layers
The package-by-feature style still honors the idea of separating layers, but that separation is implemented using separate classes. The package-by-layer style, on the other hand, implements that separation using both separate classes and separate packages, which doesn't seem necessary or desirable.

Minimizes Scope
Minimizing scope is another guiding principle of lasting value. Here, package-by-feature allows some classes to decrease their scope from public to package-private. This is a significant change, and will help to minimize ripple effects. The package-by-layer style, on the other hand, effectively abandons package-private scope, and forces you to implement nearly all items as public. This is a fundamental flaw, since it doesn't allow you to minimize ripple effects by keeping secrets.

Better Growth Style
In the package-by-feature style, the number of classes within each package remains limited to the items related to a specific feature. If a package becomes too large, it may be refactored in a natural way into two or more packages. The package-by-layer style, on the other hand, is monolithic. As an application grows in size, the number of packages remains roughly the same, while the number of classes in each package will increase without bound.

If you still need further convincing, consider the following.

Directory Structure Is Fundamental To Your Code

"As any designer will tell you, it is the first steps in a design process which count for most. The first few strokes, which create the form, carry within them the destiny of the rest." - Christopher Alexander

(Christopher Alexander is an architect. Without having worked as programmer, he has influenced many people who think a lot about programming. His early book A Pattern Language was the original inspiration for the Design Patterns movement. He has thought long and hard about how to build beautiful things, and these reflections seem to largely apply to software construction as well.)

In a CBC radio interview, Alexander recounted the following story (paraphrased here): "I was working with one of my students. He was having a very difficult time building something. He just didn't know how to proceed at all. So I sat with him, and I said this: Listen, start out by figuring out what the most important thing is. Get that straight first. Get that straight in your mind. Take your time. Don't be too hasty. Think about it for a while. When you feel that you have found it, when there is no doubt in your mind that it is indeed the most important thing, then go ahead and make that most important thing. When you have made that most important thing, ask yourself if you can make it more beautiful. Cut the bullshit, just get it straight in your head, if you can make it better or not. When that's done, and you feel you cannot make it any better, then find the next most important thing."

What are the first strokes in an application, which create its overall form? It is the directory structure. The directory structure is the very first thing encountered by a programmer when browsing source code. Everything flows from it. Everything depends on it. It is clearly one of the most important aspects of your source code.

Consider the different reactions of a programmer when encountering different directory structures. For the package-by-feature style, the thoughts of the application programmer might be like this:

"I see. This lists all the top-level features of the app in one go. Nice."
"Let's see. I wonder where this item is located....Oh, here it is. And everything else I am going to need is right here too, all in the same spot. Excellent."
For the package-by-layer style, however, the thoughts of the application programmer might be more like this:
"These directories tell me nothing. How many features in this app? Beats me. It looks exactly the same as all the others. No difference at all. Great. Here we go again..."
"Hmm. I wonder where this item is located....I guess its parts are all over the app, spread around in all these directories. Do I really have all the items I need? I guess we'll find out later."
"I wonder if that naming convention is still being followed. If not, I will have to look it up in that other directory."
"Wow, would you look at the size of this single directory...sheesh."
Package-By-Layer in Other Domains is Ineffective

By analogy, one can see that the package-by-layer style leads to poor results. For example, imagine a car. At the highest level, a car's 'implementation' is divided this way (package-by-feature) :

safety
engine
steering
fuel system
and so on...
Now imagine a car whose 'implementation' under the hood is first divided up according to these lower level categories (package-by-layer) :

electrical
mechanical
hydraulic
In the case of a transmission problem, for example, you might need to tinker around in these three compartments. This would mean moving from one part of the car to another completely different one. While in these various compartments, you could 'see' items having absolutely nothing to do with problem you are trying to solve. They would simply be in the way, always and everywhere distracting you from the real task at hand. Wouldn't it make more sense if there was a single place having exactly what you need, and nothing else?
As a second example, consider a large bureacracy divided up into various departments (package-by-feature):

front office
back office
accounting
personnel
mail room
If a package-by-layer style was used, the primary division would be something like :
executives
managers
employees
Now imagine the bureacracy being divided physically according to these three categories. Each manager is located, for example, with all the other managers, and not with the employees working for them. Would that be effective? No, it wouldn't.
So why should software be any different? It seems that package-by-layer is just a bad habit waiting to be broken.

The example applications that come with WEB4J uses the package-by-feature style.

http://www.javapractices.com/topic/TopicAction.do?Id=205...
