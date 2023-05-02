Download Link: https://assignmentchef.com/product/solved-comp2300-assignment1-part-2
<br>
This assignment has two parts: in Part 1, you need to generate a specific (simple) signal to be output through your discoboard’s headphone jack. In Part 2, you get to be creative and program your discoboard to make a more interesting sound. Your discoboard has a standard 3.5mm headphone jack, which means that any 3.5mm headphones will work.

<img decoding="async" alt="3.5mm Headphone Jack" data-recalc-dims="1" data-src="https://i0.wp.com/cs.anu.edu.au/courses/comp2300/assets/deliverables/01-synth/headphone-jack.jpg?w=980&amp;ssl=1" class="lazyload" src="data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==">

 <noscript>

  <img decoding="async" src="https://i0.wp.com/cs.anu.edu.au/courses/comp2300/assets/deliverables/01-synth/headphone-jack.jpg?w=980&amp;ssl=1" alt="3.5mm Headphone Jack" data-recalc-dims="1">

 </noscript>

<p class="warn-box">To complete this assignment you will need to have a solid understanding of the course material provided in the <a class="acton-tabs-link-processed" href="https://cs.anu.edu.au/courses/comp2300/labs/01-intro/">week 1</a>, <a class="acton-tabs-link-processed" href="https://cs.anu.edu.au/courses/comp2300/labs/02-first-machine-code/">week 2</a> and <a class="acton-tabs-link-processed" href="https://cs.anu.edu.au/courses/comp2300/labs/03-maths-to-machine-code/">week 3</a> labs. If you have not completed these or do not understand the content then it is <em>strongly</em> recommended that you go and complete these labs before starting on the assignment.

<h2 id="background">Background</h2>

A <a class="acton-tabs-link-processed" href="https://en.wikipedia.org/wiki/Synthesizer">synthesizer</a> (or <em>synth</em> for short) is an electronic device which produces a musical sound. It’s called a <strong>synth</strong>esizer because while an acoustic instrument uses a resonating cavity or string to produce “natural” sound waves, the synth uses digital logic to calculate and produce a <strong>synth</strong>etic signal which is amplified electronically and turned into a soundwave through a loudspeaker.

Although computers have been used for making music since they were first invented, it was in the 70s that improvements in technology led to an explosion of digital devices like synths in popular music—check out Kraftwerk’s <em>The Robots</em>.

In this assignment, you’re going to turn your discoboard into a synthesizer. The maths and physics behind this aren’t rocket science, it’s just generating values in simple patterns and writing them to a special register one-after-the-other. The key part is that it involves <strong>controlling the execution of your program in time</strong>. This might be a fairly new idea—you might not be used to worrying about exactly how fast your program runs, and if you do care about it you only care about making it run <em>faster</em>, not about making sure that the instructions run at <em>specific</em> times.

<h2 id="getting-started">Getting started</h2>

The initialisation sequence for the discoboard’s audio hardware (i.e. the headphone jack) is a little bit fiddly, so for this assignment we’ve provided some setup code to get you up and running (optionally, check out the <code>lib/audio.S</code> and <code>lib/clock.S</code> files if you’re curious).

Fork &amp; clone the <a class="acton-tabs-link-processed" href="https://gitlab.cecs.anu.edu.au/comp2300/2021/comp2300-2021-assignment-1">assignment 1 template repo</a> and open it up in VSCode as usual. There are two things you need to know about the setup code:

<ol>

 <li>The template repo contains two git branches: <code>part-1</code> and <code>part-2</code> (initially, they both point to the same commit). Both of these branches have all the setup code you need—the only difference between them will be the commits you make in doing your assignment. So, you should write your <a class="acton-tabs-link-processed" href="https://cs.anu.edu.au/courses/comp2300/deliverables/01-synth/#part-1">Part 1</a> code on the <code>part-1</code> branch, and your <a class="acton-tabs-link-processed" href="https://cs.anu.edu.au/courses/comp2300/deliverables/01-synth/#part-2">Part 2</a> code on the <code>part-2</code> branch.</li>

 <li>Although <em>your</em> code still goes in <code>main.S</code>, the template includes a couple of functions for you to call from your code: <code>init</code> and <code>BSP_AUDIO_OUT_Play_Sample</code>.

  <ul>

   <li>When you <code>bl</code> (branch with link) to the <code>init</code> function your program will execute the setup code to turn on your discoboard’s headphone jack. Your code should call this function <strong>once</strong> at the start.</li>

   <li>When you <code>bl</code> to the <code>BSP_AUDIO_OUT_Play_Sample</code> function, whatever is in the <em>lowest</em> (least-significant) 16 bits of <code>r0</code> will be “played” through the headphone jack (treated as a <strong>signed</strong> 16-bit number as shown in the picture). Your code should call this function <strong>repeatedly</strong> to generate the audio signal. Calculating exactly what that data in <code>r0</code> should be to generate the right signal is up to you!</li>

  </ul></li>

</ol>

If this is a bit confusing, head down to the <a class="acton-tabs-link-processed" href="https://cs.anu.edu.au/courses/comp2300/deliverables/01-synth/#faq">FAQ section</a> on this page—there’s a lot of answers which should help you understand. Furthermore, check out the <a class="acton-tabs-link-processed" href="https://cs.anu.edu.au/courses/comp2300/resources/faq/">sitewide FAQ</a> for answers that apply more generally to all assignments.

<h2 id="part-1">Part 1</h2>

In the first part of the assignment you need to write a program which produces an audible constant-amplitude <a class="acton-tabs-link-processed" href="https://en.wikipedia.org/wiki/Square_wave"><strong>square wave</strong></a> with a <a class="acton-tabs-link-processed" href="https://en.wikipedia.org/wiki/Frequency"><strong>frequency</strong></a> of 440Hz (440 cycles per second) and a <a class="acton-tabs-link-processed" href="https://en.wikipedia.org/wiki/Duty_cycle"><strong>duty cycle</strong></a> out of the headphone jack.

All of these properties are shown in the this picture: <img decoding="async" alt="Square wave" data-recalc-dims="1" data-src="https://i0.wp.com/cs.anu.edu.au/courses/comp2300/assets/deliverables/01-synth/square-wave-20pc-duty.jpg?w=980&amp;ssl=1" class="lazyload" src="data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==">

 <noscript>

  <img decoding="async" src="https://i0.wp.com/cs.anu.edu.au/courses/comp2300/assets/deliverables/01-synth/square-wave-20pc-duty.jpg?w=980&amp;ssl=1" alt="Square wave" data-recalc-dims="1">

 </noscript>

Using the <code>init</code> function provided, the audio output is configured to use signed 16-bit values for the signal, so a value of <code>0x8000</code> represents the “bottom” of the signal range, <code>0x0</code> represents the “middle” and <code>0x7FFF</code> is the “top”. The output sample rate (the rate at which these 16-bit values come out of the headphone jack as “sound”) is <strong>48kHz</strong>. This is all the info you need to put the right sequence of values in <code>r0</code> and branch to <code>BSP_AUDIO_OUT_Play_Sample</code> to make the music come out of the headphones.

You can see the “sample-by-sample” nature of digital audio in the picture—the square wave signal is actually just a sequence of dots—these are the values which you’ll output through your <code>r0</code> register and <code>BSP_AUDIO_OUT_Play_Sample</code> function. Remember that the value in <code>r0</code> <em>immediately</em> before this branch will be the one that comes out the headphone jack.

How will you know if you’re doing it right? You’ll need to plug your headphones in and listen! Your square wave should sound like a constant-pitch, slightly “buzzy” sound. Like below:



 <audio id="demo-sound" src="https://cs.anu.edu.au/courses/comp2300/assets/deliverables/01-synth/square-wave-20pc-duty.ogg" controls="controls" data-mce-fragment="1"></audio>

You can hear a normal square wave (without a 20% duty cycle, and of whatever pitch you want) at <a class="acton-tabs-link-processed" href="http://onlinetonegenerator.com/">onlinetonegenerator.com</a>.

Square waves (and similar waveforms) are quite popular in music as “lead” sounds, since they cut through the accompaniment so well—just listen to the opening square wave synth line in Van Halen’s Jump.

<p class="info-box">You might be asking yourself: “What is a duty cycle?”. Duty cycle refers to the percentage of the time the signal is “on” in one cycle (the “period”) of the wave. This leads to (as shown in the diagram above), a much shorter peak than the trough–as opposed to a normal square wave, when the peak and trough are of equal length (a duty cycle of 50%!).

For Part 1, marks will be awarded for:

<ul>

 <li>making a sound</li>

 <li>whether the sound has the correct frequency</li>

 <li>whether the sound has a peak-to-peak <a class="acton-tabs-link-processed" href="https://en.wikipedia.org/wiki/Amplitude">amplitude</a> of at least <em>half</em> the full <code>0x8000</code>–<code>0x7FFF</code> <a class="acton-tabs-link-processed" href="https://en.wikipedia.org/wiki/Dynamic_range#Electronics">dynamic range</a> (as depicted in the <a class="acton-tabs-link-processed" href="https://cs.anu.edu.au/courses/comp2300/deliverables/01-synth/#part-1">picture</a>)</li>

 <li>how “clean” the square wave signal is (i.e. how close is it to the picture above)</li>

 <li>how close the frequency of the wave is to 440Hz (to achieve the closest frequency, <strong>and possibility of full marks for this part</strong>, you will need to average over many periods)</li>

 <li>whether or not it has a duty cycle of 20% (<strong>to achieve the possibility of full marks for this part</strong>, you should have a value somewhere in your code we can tweak to change the duty cycle to some other value)</li>

 <li>code structure &amp; readability (<a class="acton-tabs-link-processed" href="https://cs.anu.edu.au/courses/comp2300/deliverables/01-synth/#ez-100">including comments</a>)</li>

</ul>

If you would like to view a plot of your output, then you can follow the instructions on the resources page to use the <a class="acton-tabs-link-processed" href="https://cs.anu.edu.au/courses/comp2300/resources/faq/#sample-plotter">sample plotter</a>.

<p class="warn-box">Be careful generating signals with your earphones in your ears—the discoboard can make a pretty loud signal. It’s a good idea to hit “run” on your program with your headphones <em>out</em> of your ears, and then carefully put them in your ears afterwards.

<h2 id="part-2">Part 2</h2>

In the second part, you need to generate a different signal (i.e. not a constant-frequency square wave). You can pick any periodic signal you like as long as it is audible, but here are a few ideas, in <em>approximate</em> order of increasing difficulty:

<ul>

 <li>a different base waveform from the square wave you made in Part 1 (e.g. <a class="acton-tabs-link-processed" href="https://en.wikipedia.org/wiki/Sawtooth_wave">sawtooth</a>, <a class="acton-tabs-link-processed" href="https://en.wikipedia.org/wiki/Triangle_wave">triangle</a>)</li>

 <li>a simple signal with some aspect (e.g. frequency, amplitude, waveform) which changes over time</li>

 <li>the weighted sum of multiple simpler waveforms (this is called <a class="acton-tabs-link-processed" href="https://en.wikipedia.org/wiki/Additive_synthesis">additive synthesis</a>)</li>

 <li>the <em>n</em>-sample <a class="acton-tabs-link-processed" href="https://www.gaussianwaves.com/2010/11/moving-average-filter-ma-filter-2/">moving-average filter</a> of a simple signal</li>

 <li><a class="acton-tabs-link-processed" href="https://en.wikipedia.org/wiki/Frequency_modulation_synthesis">FM synthesis</a></li>

 <li><a class="acton-tabs-link-processed" href="https://en.wikipedia.org/wiki/Wavetable_synthesis">wavetable synthesis</a></li>

</ul>

Marks for Part 2 will be awarded for a <a class="acton-tabs-link-processed" href="https://cs.anu.edu.au/courses/comp2300/resources/design-document/"><strong>design document</strong></a> describing what signal you’re generating and how you implemented it in ARM assembly language. You need to explain the <strong>“what”, “how” and “why”</strong> (<a class="acton-tabs-link-processed" href="https://cs.anu.edu.au/courses/comp2300/resources/design-document/">design, implementation, and analysis</a>) of what you have done. Although it’s ok if you don’t do something <em>super</em>-complex, we do take the <a class="acton-tabs-link-processed" href="https://cs.anu.edu.au/courses/comp2300/resources/faq/#ambition">sophistication</a> of what you have done into account.<a class="acton-tabs-link-processed" href="https://cs.anu.edu.au/courses/comp2300/resources/faq/#images-in-dd">Using images/diagrams</a> in this document to help explain what you’ve done is encouraged. Your design document must be in <strong>pdf</strong> format (2 pages of content + appendix + references) with the filename <code>design-document.pdf</code> in the top-level folder on the <code>part-2</code> branch.

5/5 - (1 vote)

<iframe frameborder="0" allowfullscreen data-mce-fragment="1" data-src="https://www.youtube.com/embed/okhQtoQFG5s" class="lazyload" src="data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw=="></iframe>