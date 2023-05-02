Download Link: https://assignmentchef.com/product/solved-cpe434-lab8-posix-signals
<br>
There are many mechanisms through which the processes communicate and in this lab we will discuss one such mechanism: ​<strong>Signals​</strong>.

Signals inform processes of the occurrence of asynchronous events. In this lab we will discuss how user defined handlers for particular signals can replace the default signals handlers and also how the processes can ignore the signals. By learning about signals, you can “protect” your programs from Control-C, arrange for an alarm clock signal to terminate your program if it takes too long to perform a task, and learn how UNIX uses signals during everyday operations.

<h1>Assignment Hints</h1>




<table width="576">

 <tbody>

  <tr>

   <td width="576">   void (​ *​ signal​ (​ int ​ sig​ , ​ void ​ (​​* ​func ​)( ​int ​)))(   int​            ​)/*signal() returns address of a function that takes an integer argument and has no return value. It requires two arguments: first is integer which is a signal name and second is the name of the functionFor example: If you want to handle ctrl+c (which is SIGINT) and want to call your function on ctrl+c, you will call function signal as follows */signal(​ SIGINT​ ,​ your_func_name​               )​ ;/*from this point on all ctrl+c will call the function that you have defined. If you want to revert back todefault, you can pass SIG_DFL as the function name for signal() call.If you want to ignore a signal completely, you can specify SIG_IGN as the function name for signal() call.*//*Ref: http://www.csl.mtu.edu/cs4411.ck/www/NOTES/signal/handler.html Ref: https://www.tutorialspoint.com/c_standard_library/c_function_signal.htm */ </td>

  </tr>

 </tbody>

</table>




<table width="576">

 <tbody>

  <tr>

   <td width="576">unsigned int ​alarm​(​unsigned int ​seconds​)/*this function generates SIGALRM signal to the calling process after ‘seconds’ secondsIf you want to do something after certain time, you can make a call to alarm(xx) so that it will generate SIGALRM after xx seconds. You need to make sure you have a functoin defined that handles SIGALRM function and you have appropriate call to signal() function.*/ int ​pause​()</td>

  </tr>

 </tbody>

</table>

/*

pause ( ) suspends the calling process and returns when the calling process receives a signal. It is most often used to wait efficiently for an alarm signal. pause ( ) doesn’t return anything useful.

*/




int kill​ (​ pid​ , ​ sig​           )​

/*

The command kill sends the specified signal to the specified process or process group. If no signal is specified, the TERM signal is sent. The TERM signal will kill processes which do not catch this signal.

<ul>

 <li>pid is the process-ID of the process to recieve the signal</li>

 <li>sig is the signal number. The effective user IDs of sending and receiving processes must be the same, or else the effective user ID of the sending process must be the super user.</li>

</ul>

If pid is equal to zero, the signal is sent to every process in the same process group as the sender. This feature is frequently used with the kill command (kill 0) to kill all background processes without referring to their process-IDs. Processes in other process groups (such as a DBMS you happened to have started) won’t receive the signal.

If pid is equal to -1, the signal is sent to all processes whose real user-ID is equal to the effective user-ID of the sender. This is a handy way to kill all processes you own, regardless of process group.

*/




<h1>Demo Code</h1>




<table width="576">

 <tbody>

  <tr>

   <td width="576">#include&lt;stdio.h&gt;#include &lt;sys/types.h&gt;#include &lt;signal.h&gt;#include &lt;unistd.h&gt;#include &lt;cstdlib&gt;//This function kills the process that makes a call to this function ​void ​kill_func​(​int killSignal​){printf​(​”Received kill signal %d
”​,​killSignal​);  printf​(​”tDying process %d
”​,​getpid​());  exit​(​0​);}//this function prints a message and changes the function to handle the SIGINT command to kill_func()void ​myFunction​(​int ​sigVal​){printf​(​”Received signal %d
”​, ​sigVal​);  printf​(​”Now you can kill me..
”​);  signal​(​SIGINT​,​kill_func​);}</td>

  </tr>

  <tr>

   <td width="576">int ​main​()  {//ignore the SIGINT signal // .. SIGINT is ctrl+c// .. SIG_IGN is signal ignore// .since we ignore the singla ctrl+c, you cannot terminate this process with ctrl+c  signal​(​SIGINT​,​SIG_IGN​);//for alarm signal, call function myFunction()// .. means when alarm goes off, myFunction will be called  signal​(​SIGALRM​,​myFunction​);//set alarm for 15 seconds from nowalarm​(​15​);printf​(​”This gets printed as soon as alarm is called
”​);//just running infinitely  while​(​1​);}</td>

  </tr>

 </tbody>

</table>

<h1>Assignment 1</h1>

Write a program in which :

<ul>

 <li>A parent process creates a child process</li>

 <li>If the user sends ctr+c to the parent process before 10 seconds from start, then the parent catches the signal and a message indicates that the system is protected. The child just ignores the signal. Print messages from both parent and child process when they detect ctrl+c signal, both of which should indicate that they cannot be killed yet.</li>

 <li>After 10 seconds, the parent process could be terminated using ctr+c. But before the parent terminates, it must send a signal to the Child and print statements stating it is trying to terminate the child process and has sent a signal. The Child then prints its process id with a message saying it receives a signal to terminate from parent and terminates. The parent then terminates printing a goodbye message.</li>

</ul>

<h1>Programming Hints</h1>

<ul>

 <li>Your parent forks a child.</li>

 <li>The parent and child both ignores ctrl+c signal</li>

 <li>After 10 seconds, when ctrl+c is received, the parent process should receive the signal, and send the kill signal to the child.</li>

</ul>

○ remember child still ignores ctrl+c command

○ maybe you need to send another signal from parent to child. Do you know how to create user defined signals?

<ul>

 <li>The child should handle the signal sent from the parent to terminate itself.</li>

 <li>The parent should terminate itself after the child is terminated.</li>

</ul>

<strong>             </strong>

<h1>Assignment 2</h1>

Create a Linux Time Bomb. This program stays idle through out and does nothing. But when a user enters ctrl+c to terminate the program, it should prints a page full of gibberish message and only quit after 10 seconds of completion of printing gibberish.

<h1>Topics for Theory</h1>

<ul>

 <li>How many inter-process communication methods have we studied until now?</li>

 <li>Mention any 3 methods of Inter Process Communication. Describe each of them.</li>

</ul>