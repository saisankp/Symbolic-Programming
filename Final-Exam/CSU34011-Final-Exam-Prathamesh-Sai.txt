% CSU34011 Symbolic Programming Final Examination
% Name: Prathamesh Sai
% Student ID: 19314123
% Exam Number: 28797

% NOTE: I have added my Prolog attempts for each question under the appropriate headings for each question. I have also added my thoughts/reasoning for each question to show effort and understanding for attempt marks purposes alongside the actual code; this is denoted by "THOUGHTS:" in comments under each attempt. To maximise partial credit, I have also added comments to my actual Prolog code to explain my thought process.

% Question 1

% Q1 (a) (i)

% ANSWER: As discussed in lectures, the predicate =/2 is unification without the occurs check. Hence, the X such that X=[0,X] is indeed a list as it is incapsulated within square brackets, and initially we think that the members are 0 and X. However to check this further, one can look up the query "X=[0,X], member(X, Y)." to see the members of the list [0,X]. We get an output implying that the members are actually infinite, as one output could be "Y = [[0, X]" meaning the length of the list is 1. Another is  "Y = [[0, X]|_1764]", with _1764 as an anonymous value, hence with a length of 2. Then we can get something like "Y = [[0, X], _1454, _1460]" hence with a length of 3 and this continues forever. Therefore, the members are infinite, but one of the members will always be [0,X].

% Q1 (a) (ii)

% ANSWER: As discussed in lectures, the predicate =/2 is unification without the occurs check. Hence, the Y such that Y=[0|Y] is indeed a list as it is incapsulated within square brackets, and initially we think that the members (i.e. elements) of the list are unknown and we do know that 0 is the head of the list, and Y is the tail of the list. However to check this further, one can look up the query "Y=[0|Y], member(Y, X)." To see the members of the list [0|Y]. We get an output implying that the members are also infinite, as one output could be "X = [_S1], % where _S1 = [0|_S1]," with a length of 1, and another could be "X = [_S1, _1448], % where _S1 = [0|_S1]," with a length of 2 where _1448 is an anonymous value, and this continues on forever. Therefore, the members are infinite, but one of the members will always be [0|Y].

% Q1 (b)

membe(A, B, C) :- membe(A, B, 0, C). %A is the count, B is the list, C is the result.
membe(_, [], Accumulator, Accumulator). %Use tail recursion.
membe(A, [A|Dr], Accumulator, C) :- AccumulatorIncremented is Accumulator + 1, membe(A, Dr, AccumulatorIncremented, C). % unification occurs with the head of the list and the term we are looking for, we increment the accumulator.
membe(A, [D|Dr], Accumulator, C) :- dif(D, A), membe(A, Dr, Accumulator, C). % For backtracking in our solution, we use dif/2 that is only true if the parameters are different terms.

% THOUGHTS: When I first read the question, an accumulator came to mind straight away. It was the simplest approach to the problem. I tried to use tail-recursion to be as efficient as possible. I used dif with arity 2 in my solution to keep it as short and concise as possible. It covers the first 2 cases in the question, as well as the extra case at the end for full marks. I have added comments above for explanations in each line. 

% Q1 (c) (i)

% ANSWER: TRUE. sublist and sublis2 give the same answers to every query because they use the same technique with append to check if SubL is a sublist of L, the anonymous variable "_" is simply placed in a different parameter but we will eventually get the same answer.

% Q1 (c) (ii)

% ANSWER: TRUE. sublist and sublis3 also gives the same answers to every query because they use the same techniques with append to check if SubL is a sublist of L. It switches the parameters SubL, S and L with sublis2, but will also end up with the same answers.

% Q1 (c) (iii)

sublist(SubL,L) :- append(_,S,L), append(SubL,_,S). % original sublist
sub(SubL, A) :- sublist(SubL, A), remove_duplicates(A,A).       % modified sub

remove_duplicates([], []). % base case

%remove duplicates by using member with cut to stop backtracking.
remove_duplicates([Head | Tail], Result) :- member(Head, Tail), !, remove_duplicates(Tail, Result).

%remove duplicates using recursion with the tail of the parameters.
remove_duplicates([Head | Tail], [Head | Result]) :- remove_duplicates(Tail, Result).


% THOUGHTS: I tried to get the result of the original sublist, and remove any duplicates. This caused some issues, as it worked on its own if I tried to remove duplicates from a list in the query window, but I tried to "AND" it into the clause but it wouldn't work. I have attached comments above.

% Q1 (d)

% ANSWER: TRUE. The number of solutions to the query is the instantiation of N that answers the query "findall(X,q(X),L), length(L,N)." We know that findall gets all the possible outputs of the predicate passed into it. If we have q(X). In our knowledge base, then the findall query gives back a solution of L = [_1768] where N=1, but if we add another statement to our knowledge base stating "q(X) :- true.", then we get "[_1768, _1774]," where N=2. We can determine that the number of solutions to the query is the instantiation of N.

% Question 2

% Q2 (a) (i)

int(0).
int(N) :- next_int(1, N).
next_int(I, I).
next_int(I, K) :- J is I + 1, next_int(J, K).

% THOUGHTS: I have a base case int(0), and then for the next int I have used is/2 to increment the value to get the next integer using tail recursion.

% Q2 (a) (ii)

intgr(0).
intgr(N) :- next_intgr(1, N).
next_intgr(I, I).
next_intgr(I, -I).
next_intgr(I, K) :- J is I + 1, next_intgr(J, K).

% THOUGHTS: I have a base case intgr(0), and then for the next int I have used is/2 to increment the value. But this time to get both - and + for all integers other than 0, I added "next_intgr(I, -I).". A simple addition, but makes the function work perfectly with concise code.

% Q2 (b) (i)

good([0]).
good([1,0]).
good([A,B|T]) :- good([B|T]), B>0, A is -B, !.
good([A,B|T]) :- good([B|T]), B<0, A is -B+1.

% THOUGHTS: I thought using the cut predicate would eliminate any infinite processing happening between B>0, good([B|T] and A is -B. Maybe, I should have a counter somewhere, which could check how many times we have iterated through the conditions to stop the infinite condition checking, causing the stack space to be all used up.

% Q2 (b) (ii)

good --> [0].
good --> [1,0].
good --> good, B>0, A is -B.

% THOUGHTS: I tried to convert some of the original statements given in prolog form to DCG form in the remaining time I had.

