# janissary

*janissary* is a multi-player log parsing tool for Age of Empires 2 HD edition,
written in python. It is a "toy" project, so don't expect too much in terms of
reliability or functionality. It seems the format of the log file sees a lot of
churn, as updates are released for the game. This was built and tested with HD
v5.8, and may not work with any other version (older or newer).

## To install with development requirements

`pip install -e .[dev]`

## Basic usage

The package installs the `janissary` command. It has a few sub-commands, many
of which are primarily useful for debug and development. The most useful are
probably:

`janissary report logfile.aoe2record logfile_report.html` to write an HTML
report from the log.

`janissary command-yaml logfile.aoe2record commands.yaml` will write out the
details of all of the logged commands in a YAML format, useful to understand
what is stored in the log, or perhaps to pass along to some other processing.

`janissary header-yaml logfile.aoe2record header.haml` will write out the data
parsed from the log header section.

## Similar projects and resources

Thankfully, I can stand on the shoulders of those who came before me.

https://github.com/stefan-kolb/aoc-mgx-format/ is a ruby project with good
documentation of the body section format.

https://github.com/goto-bus-stop/recanalyst is a PHP project with good
documentation of the header format.

The two of these got me most of the way there, in terms of understanding
the format. I found there were a few changes in 5.8 not yet (at time of this
writing) incorporated into either of those. Namely, some extra data in the
player section of the aoe2record header, and TRAIN commands are now stored in
command ID 0x81, which I have dubbed TRAIN2.

## Project Structure

The parsing of log files is broken up into sections: the header and the body.
The header is written at the start of game, and contains information about the
game settings, players, and initial conditions. The body contains a log of all
actions taken by players. Note that there is no state information stored in
the log file, only the actions taken by players (e.g. "attack", "move",
"build", "train a unit"). This means you can't recreate the game state (e.g.
unit positions, when units die, etc) without a perfect recreation of the game
mechanics.
