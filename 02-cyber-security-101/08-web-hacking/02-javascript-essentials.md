# JavaScript Essentials
Learn how to use JavaScript to add interactivity to a website and understand associated vulnerabilities.

## Content:
<br>[Task 1: Introduction](#task-1-introduction)
<br>[Task 2: Essential Concepts](#task-2-essential-concepts)
<br>[Task 3: JavaScript Overview](#task-3-javascript-overview)
<br>[Task 4: Integrating JavaScript in HTML](#task-4-integrating-javascript-in-html)
<br>[Task 5: Abusing Dialogue Functions](#task-5-abusing-dialogue-functions)
<br>[Task 6: Bypassing Control Flow Statements](#task-6-bypassing-control-flow-statements)
<br>[Task 7: Exploring Minified Files](#task-7-exploring-minified-files)
<br>[Task 8: Best practices](#task-8-best-practices)
<br>[To note](#to-note)

## Task 1: Introduction

## Task 2: Essential Concepts
| Concept | Description |
|---------|-------------|
| Variables | 3 ways to declare; `var`, `let`, `const` |
| Data Types | Define value type a varaible can hold, e.g. string, number, boolean, null, undefined, object (arrays or objects). |
| Functions | Block of code designed to perform a specific task. |
| Loops | Run code block multiple times as long as a condition is true. E.g. `for`, `while`, `do...while`. |
| Request-Response Cycle |  Web development: When a user's browser (client) sends a request to a web server, and server responds with the requested info, e.g. webpage, data. |

<u><b>THM Questions</b></u>
| Question | Answer |
|---------|---------|
| <i>What term allows you to run a code block multiple times as long as it is a condition?</i> | loop |

## Task 3: JavaScript Overview
<ul>
<li>JS is an interpreted language; code is executed directly in the browser without prior compilation.</li>
<li>JS is primarily executed on client side; makes it easy to insepct and interact with HTML directly within the browser.</li>
</ul>

<u><b>THM Questions</b></u>
| Question | Answer |
|---------|---------|
| <i>What is the code output if the value of x is changed to 10?</i> | The result is: 20 |
| <i>Is JavaScript a compiled or interpreted language?</i> | interpreted |

## Task 4: Integrating JavaScript in HTML
| Concept | Description |
|---------|-------------|
| JS Purpose | Not usually used to render content, but it works with HTML and CSS to create dynamic and interactive web pages. |
| Internal JS | JS code directly embedded within an HTML document; can see how script interacts with HTML. Script is inserted between `<script>` tags. Can be in `<head>` section for scripts that need to be loaded before page content is rendered. Or can be in `<body>` section for scripts used to interact with elements as they are loaded on web page. |
| External JS | Create and store JS in a separate file ending with `.js` extension. Helps developers keep HTML document clean and organized. Can be stored or hosted on same web server as HTML document or stored on an external web server such as the cloud. |
| Verify internal or external JS | When pen-testing a web application, impt to check if website uses internal or external JS. Verify by viewing page's source code; Right-click anywhere on website. > View page source. |

<u><b>THM Questions</b></u>
| Question | Answer |
|---------|---------|
| <i>Which type of JavaScript integration places the code directly within the HTML document?</i> | internal |
| <i>Which method is better for reusing JS across multiple web pages?</i> | external |
| <i>What is the name of the external JS file that is being called by external_test.html?</i> | thm_external.js |
| <i>What attribute links an external JS file in the `script` tag?</i> | src |

## Task 5: Abusing Dialogue Functions
| Concept | Description |
|---------|-------------|
| One of JS main obj | PRovide dialogue boxes for user interaction and dynamically update content on web pages. Provides builty-in functions like `alert`, `prompt`, `confirm`; allow developers to display messages, gather input, obtain user confirmation. If not implemented securely, can be exploited to execute attacks like XSS. |
| `alert` function | Displays a message in dialogue box with an 'OK' button; convey info or warnings to user. |
| `prompt` function | Displays a dialogue box that asks user for input. Returns entered value when user clicks 'OK' or null if user clicks 'Cancel'. |
| `confirm` function | Displays a dialogue box with a message and 2 buttons: 'OK' that returns true and 'Cancel' that returns false. |

<u><b>THM Questions</b></u>
| Question | Answer |
|---------|---------|
| <i>In the file invoice.html, how many times does the code show the alert Hacked?</i> | 5 |
| <i>Which of the JS interactive elements should be used to display a dialogue box that asks the user for input?</i> | prompt |
| <i>If the user enters Tesla, what value is stored in the carName= prompt("What is your car name?")? in the carName variable?</i> | Tesla |

# Task 6: Bypassing Control Flow Statements
<ul>
<li><b>Control flow:</b> Order in which statements and code blocks are executed based on certain conditions.</li>
<li><b>Control flow structures to make decisions:</b> <code>if-else</code>, <code>switch</code></li>
<li><b>Loops to repeat actions:</b> <code>for</code>, <code>while</code>, <code>do...while</code></li>
</ul>

<u><b>THM Questions</b></u>
| Question | Answer |
|---------|---------|
| <i>What is the message displayed if you enter the age less than 18?</i> | You are a minor. |
| <i>What is the password for the user admin?</i> | ComplexPassword |

# Task 7: Exploring Minified Files
<ul>
<li>Minification in JS is the process of compressing JS files by removing all unnecessary characters, such as spaces, line breaks, comments, and even shortening variable names.</li>
<li>Helps reduce the file size and improves the loading time of web pages, especially in production environments.</li>
<li>Makes the code more compact and harder to read for humans, but they still function exactly the same.</li>
<li>Similarly, obfuscation is often used to make JS harder to understand by adding undesired code, renaming variables and functions to meaningless names, and even inserting dummy code.</li>
</ul>

<u><b>THM Questions</b></u>
| Question | Answer |
|---------|---------|
| <i>What is the alert message shown after running the file hello.html?</i> | Welcome to THM |
| <i>What is the value of the age variable in the following obfuscated code snippet?<br>age=0x1*0x247e+0x35*-0x2e+-0x1ae3;</i> | 21 |

# Task 8: Best practices

<u><b>THM Questions</b></u>
| Question | Answer |
|---------|---------|
| <i>Is it a good practice to blindly include JS in your code from any source (yea/nay)?</i> | nay |
