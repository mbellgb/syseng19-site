---
layout: post
title:  "Architecture Research and Algorithm Design "
date:   2016-12-3 00:30:51 +0100
categories: development
---
This page details the research we have done through the course of designing the architecture for our system. It also includes details of the design of our matching algorithm.

__MORE__

<a href="#important-decision" style="margin-top: 7px;" class="btn btn-primary">Important Decisions</a>
<a href="#algorithm-design" style="margin-top: 7px;" class="btn btn-primary">Algorithm Design</a>

<h3 class="section-header" id="important-decision">Important Decisions</h3>
This section highlights the decisions we have made as a team for future development of this project. These decisions include the technology and tools used to build the final system.

<div class="table-responsive">
	<table class="table table-bordered decisionTable">
		<tbody>
			<col width="15%">
  			<col width="85%">
			<tr>
				<td width="100px;"><strong>Decision 1</strong></td>
				<td>Which web application framework do we use for the <strong>front-end?</strong></td>
			</tr>
			<tr>
				<td rowspan="3"><strong>Option Considered</strong></td>
				<td>Angular JS and Ionic</td>
			</tr>	
			<tr><td>React JS</td></tr>
			<tr><td>Ember JS</td></tr>
			<tr>
				<td><strong>Final Decision</strong></td>
				<td>Angular JS and Ionic</td>
			</tr>
			<tr>
				<td><strong>Justifications</strong></td>
				<td>
					<strong>Why not React JS?</strong>
					<p>We considered React JS due to its ability to build components and functionality which are well maintained. React is also able to render on server side which makes integration with backend easier. However, React is not a full framework as there are no router nor model management libraries built into ReactJS. Hence writing components is not as easy.</p>
					<strong>Why not Ember JS</strong>
					<p>We chose Ember JS as it is a flexible framework which speeds up the performance of our application without reloading the whole page. Ember is also good for REST API querying which is what we plan to use for communication with database. However, we have to write our own ajax requests since a lot of ember addons are ports of existing jQuery libraries and very few are written from scartch.</p>
					<strong>Why Angular JS and Ionic</strong>
					<p>The main advantages of Angular is the flexibility and support by the community. Angular JS is supported by Google and has a great development community. It also operates directly on DOM rather than inner HTML more efficiently. Our Client also heavily emphasised that the application should be available on mobile. Ionic is able to produce applications that can run on both IOS and Android environment. It also makes developing with Angular easier because modules from Angular ca be used.</p>
				</td>
			</tr>
		</tbody>
	</table>
</div>

<div class="table-responsive">
	<table class="table table-bordered decisionTable">
		<tbody>
			<col width="15%">
  			<col width="85%">
			<tr>
				<td width="100px;"><strong>Decision 2</strong></td>
				<td>Which web application framework do we use for the <strong>back-end?</strong></td>
			</tr>
			<tr>
				<td rowspan="4"><strong>Option Considered</strong></td>
				<td>Django</td>	
			</tr>
			<tr><td>Ruby On Rails</td></tr>
			<tr><td>Node JS</td></tr>
			<tr><td>Revel</td></tr>
			<tr>
				<td><strong>Final Decision</strong></td>
				<td>Django</td>
			</tr>
			<tr>
				<td><strong>Justifications</strong></td>
				<td>
					<strong>Why not Ruby on Rails?</strong>
					<p>We considered Ruby on Rails as one of our options due to the expressive nature of the ruby language, allowing funtions to be written very fast. Ruby also had a large developer following and library. However, due to scaffolding could mean that ou application is inflexible and also the dynamic nature of Ruby meant that it runs a little slower compares to statically typed languages.</p>
					<strong>Why not Revel</strong>
					<p>Revel was also an option due to its similarity to Ruby on Rails. Revel was written using the Go language which is statically and strongly typed language. Go has strong concurrency and has many multithreading features. However, Revel may not be the most suitable framework due to being too lightweight. This meant that we had to implement a lot features ourselves, making development inefficient.</p>
					<strong>Deciding between Node JS and Django.</strong>
					<p>After some discussion the team agreed that Node JS and Django was the most suitable for this application. However, we it was hard to decide between the two frameworks. Node JS is extremely popular over the last cople of years since the community is large and there is easily to find resources Node JS also shares a common language with web page scripts as Node JS programs are written in Javascript. Node applications are also Event-driven which means it can perform other tasks while wating for asynchronous operations.<br>
					On the other hand, Django is a very high level framework written in python which could lead to rapid development. Django takes care of most aspects of web development such as user authentication and also has an in built admin system. The scalabitlity of Django also makes it very suitable for our application since there are a reasonable amount of users that could potentially use this application.<br>
					Ultimately, the team decided on Django as the framework for the backend as we felt that Django had many in built systems that could be useful to our application, making development the most efficient.</p>
				</td>
			</tr>
		</tbody>
	</table>
</div>

<a href="#top" class="btn btn-primary">^ Back to top</a>

<h3 class="section-header" id="algorithm-design">Algorithm Design</h3>
<span class="lead sub-header">Abstract</span><br>
**Input**: The algorithm will accept  two lists, Mentors and Mentees, containing users. Users have a name, as well as a list of tags.<br>
**Final Output**: A list of matches between mentors and mentees.

<span class="lead sub-header">Process</span><br>
**Stage 1: Ranking**<br>
The algorithm will iterate through each mentee in turn: It will look at each mentor, and assign a ‘score’ between the mentee and mentor, which begins at 0. For each matching tag, the score should be increased by 10. Next, it should consider the constraints of the current mentorship programme. For each match in the mentorship programme (e.g., preferences on time served in company), the score should be increased by 5. If the match should absolutely not happen for whatever reason, the score should be set to 0 and the matching algorithm should move onto the next mentor.

By the end of this process, for each mentee, there should be a list of mentors and their scores. These should be sorted by the highest score first.

How can we make this faster? Potentially we could cache some of the scores to adapt to similar matches, but for now it’s better to just write a brute force approach and optimise later if need be. To begin with, we should allow users to input tags for themselves. In the future though, we could use semantic analysis to find these tags automatically from their user bio.

**Stage 2: Selecting**<br>
Each mentee then will be presented with their top three mentors. They will be invited to sort these according to preference (within a time limit), and once these have all been completed, the next stage will begin.

How will we implement this in reality? Most likely, we’ll use the first three values from each mentee’s matches/scores array and sort them in place to save memory.

**Stage 3: Matching**<br>
The algorithm will now implement a version of the Gale-Shapley Algorithm to find an optimal set of matches given the mentee’s preferences. The difference here is that there is most likely an uneven amount of mentees to mentors. It’s worth noting at this point that mentors may be after more than one mentee; in this case, the score arrays need to be adjusted so that a mentor with x spaces has x values in the arrays.

Once this algorithm is run, the matches are then sent to the DB to be saved.

<span class="lead sub-header">Pseudocode</span><br>
Below is the pseuodocode lllustrating the the 3 stages explained above. A user class is used to store the ID of the mentee or mentor, along with the required tags needed for matching.

<img src="{{ site.baseurl }}/assets/img/pseuodocode.jpg" class="image" alt="node.js" height="80%" width="95%"><br>

<a href="#top" class="btn btn-primary">^ Back to top</a>
