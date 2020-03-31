---
layout: post
title:      "Performance Review: React/Redux"
date:       2020-03-31 22:03:40 +0000
permalink:  performance_review_react_redux
---


What's this about?
I had my "final" project assessment, which covered React and Redux, this past Friday. I ran over the 45 minute timeframe, and will need to complete the remainder of the assessment early in the upcoming week.

Rather than be upset about it, I thought I'd do the next best thing: analyze it. Here I'll be covering both the bright spots and the pain points, as well as giving the major highlights in the hopes that they might be helpful for you, fellow dev.

Lesson Learned #1
Demonstrate your understanding without becoming verbose.
During my assessment, I was asked a simple question:

Why is dispatch so important in redux?

A simple question deserved a simple answer, something short and elegant:

The dispatch function sends an action to the reducer.

If I wanted to get fancy, adding

it is the only way to trigger a state change,

would have been perfectly acceptable as well. Boom, done. That would have been a total of perhaps 10 seconds. My answer took much longer, and I found myself giving the snake legs, invariably turning it into a lizard when the assessor asked for a snake.

Simply put, while I understood what dispatch did, I spoke so much that not only did it seem like I was speaking in the hopes that the right answer was somewhere in my explanation, but also so much that it appeared as if I did not know that answer at all.

This happened several times during the assessment, which led to us running out of time.

My assessor was kind enough to chalk it up to nerves.

Lesson Learned #2
Just because you made it work, doesn't really mean you understand WHY it works.
Okay, so this lesson is one I learn time and time again. It's easy as a new developer to think you know why things are working simply because they do. In my case, I thought I fully understood the connect() function because I had used it.

I knew, for example, that connect() connects us to the store, which holds global state.

I knew that the connect() function took in several optional parameters, the first being mapStateToProps, the second mapDispatchToProps, as well as two others that I haven't yet used, such as mergeProps and options.

I also knew that if a component did not need access to the store to pass state down to props, I'd need to pass in null as the first parameter.

What I didn't fully understand was what was happening as I handed an action-creator function to connect.
import { connect } from 'react-redux';
import {getDecks} from '../actions/decksActions';

this.props.getDecks();

export default connect(mapStateToProps, {getDecks})(GetterApp);
What I thought, and subsequently stated, was that I needed to pass {getDecks} to connect to make it available for the component to use.

Even though I knew that I had made the function available for use by importing at the top of the file, I thought that passing it to connect was the only reason I was able to use it. However, I could have called the function by simply writing:
getDecks();
Doing so wouldn't have required redux at all. It also wouldn't have worked.

Passing the action creator function to connect made it possible to invoke the function by preceding getDecks(); with this.props. It also, and most importantly, bound the dispatch of the store TO the action creator.

So here, in my action creator function:
export const getDecks = () => {
  return(dispatch) => {
    dispatch({type: "LOADING_DECKS"})
    return fetch('/decks')
    .then(resp => resp.json())
    .then(decks =>
      dispatch({type: "DECKS_LOADED", payload: decks})
    )
  }
}

I HAVE ACCESS TO DISPATCH BECAUSE IT WAS ACTUALLY BOUND TO THE FUNCTION .

Lesson Learned #3
Doubting yourself out loud is a major no-no.
When my assessor asked me to guess the order that a few console.logs would be fired, the first words out of my mouth were something along the lines of "this is going to be difficult."

While this thought spoken out loud helps me stay calm and promotes me taking a deep breath, it certainly doesn't inspire confidence.

So this lesson was short- only think out loud to show your assessor how you work through a problem.

And finally, a bright spot in Lesson #4
Be proud of however far you make it
It is SO easy and SO tempting to believe that you have to be right 100% of the time, to be afraid to make mistakes, to not want to say "I don't know." Especially when you're in an assessment.

But it's okay to make mistakes and say I don't know. Ultimately, the person on the other side is there to help you succeed either as a student, or an employee. No one wants to see you fail.

By admitting the shortcomings in your understanding, or by giving a SUPER quick explanation of why you think something works even if you're wrong, you're making it possible for the person assessing you to steer you in the right direction, or simply to explain to you what part of the puzzle you're missing.

And THAT is a win in everyone's book.
