# H.E.R.A.

## High Efficiency Reactive Architecture, for Flutter  
Scott Stoll designed, Simon Lightfoot approved.

## The Story So Far...

One day in the Worldwide Flutter Freelancer's Remote Office _(A.K.A. Scott's Zoom)_, Scott and Simon were having a conversation about ValueNotifier. At one point, Scott turned to Simon and asked: 

_"If Redux is passing around the entire app state that's full of stuff we don't need, ChangeNotifier is passing around entire objects when we only need one variable, and ValueNotifier  only passes the variable we care about and nothing else... then tell me again WHY we aren't just using ValueNotifier and throwing everything else out the window??"_

After Simon silently shrugged in his secret code that means: _"I have no idea"_, Scott became stubbornly determined to use ValueNotifier in a way that passes only the data he's using, and nothing else. After he spent three days finishing the original design (and Simon spent two hours tearing it to pieces and refactoring it) H.E.R.A. was born.

The principle behind H.E.R.A. is this:
> If your Widget only uses one thing, then maybe you should only be passing it that one thing.

The way it works is actually quite simple:
 
- The UI sends events to the logic in the HeraObject.
- Each member variable we care about in the UI is wrapped in a ValueNotifier inside its HeraObject.
- The ValueNotifiers are doing the job of a "Model", in other architectures.
- When a value we care about changes, its ValueNotifier notifies its listeners in the UI.
- After the UI has updated for all changes, it sits and waits for the next event.  
  
 ## Benefits:  
 * One way, circular flow of data and events. This is an important principle borrowed from Redux. The flow is:  
 
     > 1. UI 
     >2. Logic 
    > 3. Variable that gets changed 
    > 4. That individual variable's ValueNotifier 
    > 5. Back to UI 

Here are the steps in more detail:
1. UI events call functions in the "HeraObject" logic section. (The HeraObject is just your object.)
2. Those functions (logic)  change values of member variables in the HeraObject.
3. A variable being changed triggers its ValueNotifier. Here, a ValueNotifier does the job of a "Model".
4. Each ValueNotifier has one or more ValueListenableBuilders. Every time their ValueNotifier sends the signal, these will *automatically* rebuild whatever child you gave them, using the new variable values.  
  
## Compared to Redux
- Redux passes the entire app state around. HERA only passes the variable you care about.
-  Boilerplate. The entire self-contained HERA example app is only 98 lines, including
        main( ), imports and whitespace.
- Much more...  


## Compared to ChangeNotifier
   - ChangeNotifier passes the entire object that extended ChangeNotifier.
   - ChangeNotifier could cause unnecessary rebuilds if you aren't careful and don't use a Selector.
   -  HERA passes ONLY the individual variable or object that changed.
             You *can't* get an update for something you don't care about, it's just not possible.
             Therefore, it's impossible to trigger unnecessary rebuilds. You don't need to plan carefully and there's no more need for Selectors.


## Compared to BLoC
   - With HERA, the app-wide logic should still be placed in its own folder and files.
   - Logic that applies only to a specific HeraObject should be located in that HeraObject.
   - NO STREAMS
        - No need to worry about subscriptions.
        - No need to worry about single-use vs. broadcast streams.
        - You will never forget to close a stream again.
  ---
A heavily commented example app starts at main.dart main( )  

... or...  

A very simple example without comments is in the self-contained example. To run it, be sure to use it's main function as your app entry point instead of the main.dart file's one.
