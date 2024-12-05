# Slime50
#### Video Demo:  https://www.youtube.com/watch?v=jTOeH1Tb81U
#### Introduction
My project is a web application which can be accessed here: https://rupertwatson.github.io/Slime50/.

I included all of the HTML5, javascript, and CSS in one single HTML file "index.html".

My web application was created to simulate the motion of a slime mould in a 2d space. I was inspired by a video by Sebastian Lague :Coding Adventure: Ant and Slime Simulations https://www.youtube.com/watch?v=X-iSQQgOd1A). I wondered if it was possible to simulate the behavior of a slime mold within a browser based simulation. My aim was to make the simulation as interactive as possible and easy to use, with the user being able to alter a wide variety of parameters. I designed a project web page in HTML5 which includes the Javascript canvas, a control panel, and a brief description of the project.

I decided to use the Javascript Canvas method after being inspired by this particle simulation https://physics.weber.edu/schroeder/md/. 

#### Simulation features
- Ability to start, stop, and reset the simulation using buttons.
- Ability to change the number of particles in the simulation using a slider from 1 to 5000.
- Ability to simulate up to 3 different species of slime that interact with eachother.
- Ability to set the starting position of the particles from a number of presets.
- Ability to change the colors of the background, particles and the particle trails, as well as toggle visibility.
- Ability to change parameters affecting the behavior of the slime mold, including particle speed and rotation.

#### Design challenges and choices
The first challenge of the project involved making the trails of the particles. It was important that they fade gradually over time. I discovered the "getimagedata" function, that obtains the rgba values of each pixel of the canvas in an array. I wrote the following code that subtracts 1 from the alpha channel of every pixel, and then prints the modified data onto the canvas. I also created 3 separate canvases (one each for the trails, particles and background) to prevent the background and particles from becoming transparent.

```javascript
                const imageData = contextTrail.getImageData(0, 0, canvas.width, canvas.height);
                const data = imageData.data;
                for (var i = 0; i < data.length; i += 4) {
                    data[i + 3] -= 1; // alpha channel
                }
                contextTrail.putImageData(imageData, 0, 0);
```
I also used the "getImageData" method to control the behavior of the particles. For each particle, I looked at specific pixels to the left and right of the particle at an angle and an offset distance, and summed the total RGB values of each side to get a measure of the strength of the "signal". If one "signal" is stronger than the other, the particle will rotate towards it.

```javascript
                                signalStrengthR += dataR[j] + dataR[j + 1] + dataR[j + 2] + dataR[j + 3];
                                signalStrengthL += dataL[j] + dataL[j + 1] + dataL[j + 2] + dataL[j + 3];
```

Another challenge involved adding multiple species of slime. At first, I considered making a seperate array of new particles, and duplicating code in order to handle the new species. However, I realised a simpler way was to use a modulo operator to ensure there were always equal particles of each species. I then modified the behavior to turn towards particles of its own color, and away from those of other colors. This meant that I could not change the colors of the trails with more than one species present.

```javascript
                            else if (numberSpecies == 2) {
                                if (i % 2 == 0) {
                                    signalStrengthR += dataR[j] - dataR[j + 1] + dataR[j + 2] + dataR[j + 3];
                                    signalStrengthL += dataL[j] - dataL[j + 1] + dataL[j + 2] + dataL[j + 3];
                                }
                                else{
                                    signalStrengthR += -dataR[j] + dataR[j + 1] + dataR[j + 2] + dataR[j + 3];
                                    signalStrengthL += -dataL[j] + dataL[j + 1] + dataL[j + 2] + dataL[j + 3];
                                }
```

I considered adding "mouse-touch" interactions to the project, with the user potentially being able to "draw" slime
on to the canvas, during the simulation or to make their own initiation mode presets. Ultimately, I wasn't able to add this in time.

#### Conclusion
I am proud of my final project, Slime50. I learnt a lot about Javascript (including canvas), particle based simulations, user interfaces, and even video editing. There's potential to adapt the code to mimic ant behavior. Finally, I'd like to thank the whole team at CS50x for providing a fantastic course.



