
### For a data of four, map hidden data based on reverse cryptography.

#### Mode One
~~~ruby
  0   1   3   7   6   4
0 0,0 0,1 0,3 0,7 0,6 0,4
1 1,0 1,1 1,3 1,7 1,6 1,4
3 3,0 3,1 3,3 3,7 3,6 3,4
7 7,0 7,1 7,3 7,7 7,6 7,4
6 6,0 6,1 6,3 6,7 6,6 6,4
4 4,0 4,1 4,3 4,7 4,6 4,4
~~~

# Vinput
Just a theoretical breakdown of Vinput.

## Future Implementation
### Create a vortex patterns of 1, 2, 4, 8, 7, 5
~~~ruby
one = 0
two = 1
fro = 3
eit = 7
sev = 6
fiv = 4
~~~

### From a selected mode of this vortex, create initialization dataset.
~~~ruby
mode_1 = [ one, two, fro, eit, sev, fiv ]
mode_2 = [ fiv, one, two, fro, eit, sev ]
mode_3 = [ sev, fiv, one, two, fro, eit ]
mode_4 = [ eit, sev, fiv, one, two, fro ]
mode_5 = [ fro, eit, sev, fiv, one, two ]
mode_6 = [ two, fro, eit, sev, fiv, one ]
~~~

### Use nueral network to infer implied data.
~~~ruby
open("_imaginedpath/outcomes/input.txt", "a") { |f|
  character_fates = File.readlines("_narratives/outcomes/character_fates.txt")
  dating_outcome  = File.readlines("_narratives/outcomes/dating_outcomes.txt")

  f.puts "#{character_fates[7]} #{dating_outcome[3]}"
}
~~~

### Classify outcome in comparison to other known outcomes.
~~~ruby
require "naive_bayes"

outcomes = NaiveBayes.new(:worst,
                      :nuetral,
                      :best,
)

## Train specific outcomes on Naive Bayes.
outcomes.train(:worst,    "[ Charlotte Dies ] [ Never Dates Player ]",   "worst")
outcomes.train(:nuetral,        "[ Charlotte Dies ] [ Dates Player ]", "nuetral")
outcomes.train(:nuetral, "[ Charlotte Lives ] [ Never Dates Player ]", "nuetral")
outcomes.train(:best,          "[ Charlotte Lives ] [ Dates Player ]",    "best")

outcome_file = File.readlines("_imaginedpath/outcomes/input.txt")
row      = 0

outcome_rotation = outcome_file.size.to_i

## This way you never have to worry about how long your input file ends up becoming.
outcome_rotation.times do
  sleep(1)

  chosen_outcome = outcome_file[row]

  result = outcomes.classify(*chosen_outcome)

  print result
  puts " "

  row = row + 1
end
~~~
