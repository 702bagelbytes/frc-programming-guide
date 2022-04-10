# FRC Programming - Guide
By Spencer Pogorzelski - FRC 702 "Bagel Bytes"

## Note - Work in progress!

This guide is not yet finished. 

## Purpose

This guide is intended to enable the reader to teach themself to program FIRST Robotics Competition robots. It is intended to show how to find knowledge - not to repeat it directly, to prevent it from becoming outdated. Whenever possible, this guide defers to the official documentation - therefore you need to read the links when the guide says to so that you effectively learn. 

## Should you program FRC robots?
Yes! Programming FRC robots does not requires only basic math and programming knowledge. The [WPI Library](https://docs.wpilib.org/en/stable/index.html) for programming FRC robots makes it easy, and there is plenty of documentation online about how. You need to put together various resources to teach yourself. This guide serves to help point you in the right direction, and hopefully save you time, however, it may not be the best way for you. If your team has veteran programmers, they can help you get a jump start on learning, but if you find yourself struggling to understand or trying to "memorize" lines of code, this guide may help you understand the "why" behind programming and help you form your own understanding. 

## Getting Started - Getting your robot driving

You might find it helpful to read the whole step before doing anything. 

1. First, you should decide on a programming language. The main options are Java and C++. There is also a Python library (that uses the C++ library internally), but it usually isn't ready right after kickoff, and generally adds more complexity, so I wouldn't recommend it (even as a Python fan). I would recommend Java as your programming language as you don't have to worry about managing memory since Java is garbage collected. Most teams end up choosing Java. Once you choose a programming language, you have to learn it. An internet resource that might help is [freeCodeCamp](https://www.freecodecamp.org/). One tip that I would give for learning online is to understand everything you are doing when following a tutorial rather than simply copying. 
2. Aquire a robot to test with. 702 always has a "Demobot" for this. If not, your team needs to build one. WPILib has [a resource](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-1/how-to-wire-a-robot.html) on building it. 
3. Understand how the robot works, both mechanically and electronically. WPILib has [an overview of the robot's hardware](https://docs.wpilib.org/en/stable/docs/controls-overviews/control-system-hardware.html) and [software](https://docs.wpilib.org/en/stable/docs/controls-overviews/control-system-software.html) which is helpful for gaining a basic understanding. The two main companies that produce electronic hardware for FRC robots are Cross the Road Electronics (CTRE) and REV robotics. You can mix components from both companies without problems.
4. Follow the [Zero to Robot Guide](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-2/index.html), Step 2 (Installing Software) and Step 3 (Preparing Your Robot). Make sure to read and understand the articles under "Next Steps" to learn how to use your IDE, deploy code, and install 3rd-party libraries. You can always refer back to these later. 
5. Install [3rd-party libraries](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html) for your speed controllers. Pay attention to the note regarding offline installers, as you will need tools from the vendor to configure your speed controllers. 
6. The speed controllers used at the time of writing are CTRE Talon SRX, CTRE Talon FX (Falcons), and REV CANSparkMax. You can configure CTRE devices using Phoenix Tuner and the REV devices using REV Client, which you can find on the manufacturer's website if you don't already have them. You can use the "Blink" feature to find the ID of each controller, changing them if necessary. The IDs are unique numbers that your code uses to identify the speed controllers. 
7. You can now follow the instructions under "Step 4: Programming your Robot" to understand the structure of a `TimedRobot`.
    - Instead of using `PWMSparkMax` objects and a `DifferentialDrive`, create motor objects matching the speed controllers on your robot. Instructions can be found by searching for your speed controller's name on Google. Usually this consists of writing the speed controller's type and ID.
    - Instead of a `DifferentialDrive`, create a drive based on the drive type for your robot. For Differential/Tank/West-Coast Drive (they are all the same) and Mecanum Drive, refer to [the WPILib Docs](https://docs.wpilib.org/en/stable/docs/software/hardware-apis/motors/wpi-drive-classes.html). For swerve drive, it's probably a little advanced for a beginner but you should be able to find some libraries on the internet. You usually need to invert one side of the drivetrain (because the motors are mounted opposite ways), and this is typically done in the constructor. 
    - Instead of trying to program auto first like the tutorial says, try to program teleop by connecting the drivetrain to a joystick. You need to [set up your controllers](https://docs.wpilib.org/en/stable/docs/software/basic-programming/joystick.html) with WPILib. It is easiest for both the programmer and driver to use Xbox or PS4 controllers, since they are easy to obtain, program, and have many buttons. 702 uses two Xbox controllers: one for driving and one for the co-driver to control mechanisms. 
    - Next, you must set up your driving scheme by connecting joysticks to your drivetrain's inputs. The link for setting up your drive shows what choices you have for drive schemes. At the time of writing, mecanum has arcade drive through `driveCartesian`, while tank has `tankDrive` and `arcadeDrive`. For each of the paramaters to these functions, you must provide the data received from one of your joysticks. 702 uses an `XboxController` with a split-arcade setup with a Mecanum drivetrain. This means that the left joystick X and Y control movement and the right joystick X controls rotation, but you can set up your controls however you and/or your driver wish.
    - If this seems all over the place, that is because it is! By combining all of these resources, you should be able to get a working robot program. Once you get your robot driving, the hardest part of programming is behind you!

## Going Further - Command Based Programming

Once you have your robot driving, you will want to move on by programming robot subystems and autonomous code. Before you do so, I encourage you to learn about Command-Base programming. First, read the article [What is "Command-Based" Programming](https://docs.wpilib.org/en/stable/docs/software/commandbased/what-is-command-based.html) for a description of what it is and why it is useful. It may see weird and "bloated" to create a new class for each of your robot's subsystems and functions, but once you try it, you can never go back. 
A couple benefits of command-based programming are:
- keeping the code organized: many files are easier to navigate through than one long `Robot.java` file, plus you can have more than one open at a time
- you can easily bind commands to controller buttons with just a couple lines of code using a `JoystickButton`
- you can compose complex autonomous routines by `SequentialCommandGroup`
- Lots of convience methods, like you can timeout commands with a simple `.withTimeout(delay)`
- I really like [Functional Programming](https://en.wikipedia.org/wiki/Functional_programming), and the functional "lifecycle" of commands makes sense
- the system of "requiring" subsystems makes sense and helps catch bugs

