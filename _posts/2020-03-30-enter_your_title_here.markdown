---
layout: post
title:      "A CLI Project Built With Love "
date:       2020-03-30 02:51:36 -0400
permalink:  enter_your_title_here
---

Please visit https://medium.com/@_codeBaby/a-cli-project-built-with-love-6938f444c6a8 for the full blog post with images.

![](https://miro.medium.com/max/1260/1*untSK8kBGV-NcdexYEM1uw.png)

I know what you’re thinking- “the title says “CLI Data Project… where’s the code”?
I promise we will get there, but every story starts at the beginning.
You’re looking at Duchess. She’s been up for adoption for the last 43 weeks and no one knows why. I’ve been following her on Instagram for months now, and every single day that passes is another day I wish that I could take her in.
I often spend my time thinking about her, half hoping no one has adopted her so I can adopt her myself, half hoping her picture will stop appearing in my timeline, signifying that she’s found a new home.
This project is dedicated to her.
If code is supposed to solve real world problems, maybe this small work of love will get Duchess into a home where she can love and be loved.
If you’d like to learn more about Duchess and her canine pals, and/or learn about how Paws4Life K9 Rescue also saves lives by helping prison inmates receive sentence commutations, please visit https://pawsforlifek9.org/
And now to our scheduled program.
1. The Setup:

Getting my local environment ready for the project was ground zero. Many students have used the Bundler gem to create a project repository, but I found it produced many unnecessary files for my project’s purpose. I chose to configure my files manually.
My project folder includes a bin folder that houses my executable file(dogs), and the config folder, which houses environment file.The environment file is home to all of the program’s required files and gems. The gems must be downloaded onto the computer on which the program will run. From the command line (your terminal), you need only type:
gem install <gem_name>
If you come across an error message that states that you do not have write permissions to install the gems (as I did), it is possible to solve the issue by instead typing the following into the terminal:
sudo gem install <gem_name>
Lastly, I established a lib folder that holds each class in a separate file. It is important to separate them based on class responsibility. The CLI class handles the logic, the Dog class instantiates Dog objects, and the Scraper class scrapes the data that is handed to Dog objects to give them attributes.
Note: the initial setup involved only an executable file and the three class files. Once the word on those files was done, an environment file was added, and file dependencies were moved into their appropriate places.
2. The Dog Class:
I started the project in the most logical place- creating dog objects. If the whole program is about showing what dogs are up for adoption, and giving the user information about them, it makes sense that we want to create them first, and then think about how to give them attributes.Creating this model was pretty standard. Defined the class, gave it the ability to set and get (or write and read) attributes for each dog object (or instance), and provided a class variable @@all set to an array- so that the class can hold all of its dog objects. Each dog object saves itself to the array upon initialization, as defined by the initialize method. The class method self.all returns all of the dog objects with all of their attributes.
We will need to access the dog objects in our other classes.


2. The Scraper Class
Our Scraper class “scrapes” the desired webpage to acquire the information that is necessary to the application. Recall that both the Nokogiri gem and Open-Uri gems are part of our program’s requirements. Open-Uri allows the program to open the url as if it were a file, while Nokogiri grants the ability to parse HTML and XML. Without these, our program is unable to obtain the information that we need for our program!

As you can see, I have set the variable ‘doc’ to a url that is being opened as a file by open-uri and parsed with nokogiri. Now we can get to selecting what we need by using css selector methods. But how do we do that?
If you right click on a webpage and select “Inspect Element” in the drop down menu, you’re given a set of developer tools to play with within the browser. There are two ways to approach the task of finding our selectors. The first is to press the cursor-in-box button at the top left hand corner of the developer tools pane, then using our cursor to float over the elements on the page. Our job is hard enough, so make your life easier by using the elements inspector in the developer tools pane. By moving the cursor over the element’s css code, the element itself is highlighted on the page!

In the example above you can see how the first class “fl-post-grid” denotes the container for all of our dogs, and each dog is contained in the class “fl-post-column”. So what can we do with this information? We can iterate.
The syntax:

What next? Let’s make some dogs to pass the information to!

Line 7 makes a new dog object.
Line 8 gives our dog a name based on the information obtained from our scrape, while line 9 gets a short description, and 10 gets the profile url.
Neat! Those attributes were set. Is our dog holding on to those? You bet! Think back to our Dog class:

Our attr_accessors are both readers and writers. When we write:
doggo.name= dog.css('a').text.strip
.name isn’t something magical that just gave the dog it’s name.
It’s a method! The attr_accessor :name is doing two things:
Setting/writing the name instance variable from the name variable that is being passed into the method:
def name=(name)
@name=name
end 
& getting/reading the name instance variable:
def name
@name 
end 
Magical, but not quite magic. The other attr_accessors function in this same way, setting instance variables in setting and getting methods that can be called upon in other parts of our program.
The rest of the scraper class follows the same process for gathering data passed to the dog object. We’ll come back to the second method later, however, to demonstrate how the dog object that already existed is passed into it to set other object attributes.
3. The CLI Controller
The CLI controller is responsible for controlling the program’s logic, taking in user inputs, and presenting program outputs. The CLI controller does NOT scrape data, and does NOT give our dog objects any attributes.

Looking at this code (which will be refactored at some later point in time), it’s obvious that it is styling that the end user sees.
sleep(x) produces a short delay so that not all output appears at all at once.
Series of underscores and dashes provide division to make it easier for the user to see and understand the information presented.
Scraper is called within the run method to get the information needed for the next method, list_dogs (which is established in the lower portion of this image).
The program outputs some instructions, and then takes an input, stripping it of whitespace- leading and trailing- and downcasing it (strange, but remember, our user is allowed to input strings as well as integers).
The while loop is established and given an If-Elsif statement to handle different input cases. Validations are put into place so that our program runs properly even if given an erroneous input (negative numbers, numbers that exceed the range, random strings).
As you can see, nothing is set by our CLI class, it merely calls upon other methods to run.
If you look closely enough, you might think- but what about the second scrape, Scraper.dog_details ? Aren’t you setting something there?

Well… no. Here’s what’s happening:
Our program gets an input, and if that input converts to an integer between 1 and the length of our @@all dogs array (from our Dog class), it sets a local variable dog to Dog(the class).all(the dogs)[at the index which equals our users input, converted to an integer minus one(because arrays start at index 0)].
Then it scrapes dog details. Our program switches tasks and jumps from our CLI class, to our Scraper class.That dog variable (which, remember, is an object that was previously created), is handed to the Scraper class method dog_details . The urls are passed in as files, the html is parsed using nokogiri, and our passed in dog object is given it’s attributes using attr_accessors. Magic? Magical.

The program executes this code, and jumps back to the CLI class, and continues to execute the rest of the program.
It’s important to note that scraping a few times is fine, but excessive scraping can lead a website to block you. Make sure to scrape only once. Easy way to do this is to make sure that you are not including the scrape as part of a loop (as with the first scrape) or including a validation. Our scrape below only happens IF it hasn’t happened once before.

Final Touches
The ‘colorize’ gem was installed and required by our program environment. Colorize gave the program a nice pop of color that makes the terminal less dull.
The config folder was created, and dependencies were moved to their appropriate places.
Here is the final product!

A special thank you to Nancy Noyes, our cohort lead for the Flatiron School Full-Time Online Software Engineering Program, for her extensive help, and to the Flatiron school for an incredible first module in the curriculum.
Other honorable mentions: Stefanie Davis, Victoria Hale, and Huda Usif for their enduring support and willingness to critique, guide, and help as I moved through the process of building this project.

