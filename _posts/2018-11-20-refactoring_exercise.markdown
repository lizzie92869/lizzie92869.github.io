---
layout: post
title:      "Refactoring exercise"
date:       2018-11-20 18:49:32 +0000
permalink:  refactoring_exercise
---


I recently have been given a refactoring exercise I thought I would share with you. If you are looking for a little challenge read on!

"Please refactor the implementation. Clarity and duplication are a given, flawed implementation is also likely.
Leave a note about what you refactored and why. Calling specific named smells and specific named refactorings should be the norm."
*This refactoring example is a ruby port of a refactoring exercise from Extreme Programming Explored. template.rb taken from the the Ruby refactoring code https://github.com/kevinrutherford/rrwb-code*

```
module Template
  def template(source_template, req_id)
    template = String.new(source_template)

    # Substitute for %CODE%
    template_split_begin = template.index("%CODE%")
    template_split_end = template_split_begin + 6
    template_part_one =
      String.new(template[0..(template_split_begin-1)])
    template_part_two =
      String.new(template[template_split_end..template.length])
    code = String.new(req_id)
    template =
      String.new(template_part_one + code + template_part_two)

    # Substitute for %ALTCODE%
    template_split_begin = template.index("%ALTCODE%")
    template_split_end = template_split_begin + 9
    template_part_one =
      String.new(template[0..(template_split_begin-1)])
    template_part_two =
      String.new(template[template_split_end..template.length])
    altcode = code[0..4] + "-" + code[5..7]
    template_part_one + altcode + template_part_two
  end
end
```

So how would you solve this? Give it a shot, I am waiting for you!

I am very curious to see how others would have solve this. Today, I am still wondering if the solution I brought was what was expected. After having understand what the code was really doing I solve the problem in 3 lines of code but thought afterward they might be a chance they wanted to template a pattern for any need of replacing a string by another. So in the comment of the file I added a plain english version of the logic I would have use in this case too. 

Here is my solution file
------------------------------------------------------------------------------
SPOILER ALERT: SOLUTION BELOW!
------------------------------------------------------------------------------

```
module Template
  def template(source_template, req_id)
# ---------------------------------------------------------------------------------------------------------------------------------------------------#
    # the "code smells" are :
      # duplicated code: finding the beginning and the end of a word to extract it and replace it 
      # by something else is done twice
      # Too many temporary strings (that are overwritten): template_split_begin, template_split_end, 
      # template_part_one, template_part_two
      # Uses of new.String() which create a brand new data instead of having simple data type 
      # conversion with to_s or prepending String
      # maybe the way to insert "-" in the middle of a string could be better as right now it involves
      # the concatenation of three strings. 

    # For the record: the way this code was written requires that both of the key words are present 
# ---------------------------------------------------------------------------------------------------------------------------------------------------#
    # If we are ok with regex:
    code = req_id.to_s
    altcode = code[0..7].insert(5, "-")
    # Replace %CODE% with code, and %ALTCODE% with a dashed version of req_id
    String source_template.gsub(/[%]\bCODE\b[%]|[%]\bALTCODE\b[%]/, 
      '%CODE%' => code, 
      '%ALTCODE%' => altcode
    )
# ---------------------------------------------------------------------------------------------------------------------------------------------------#
    # If we want a substitution process more generic because we will have to replace more complex 
    # patterns in the future, we could:  
    # - from this function, define code and altcode (in a string data type)
    # - from this function, call an outside function passing it the source_template as a string, 
    #   the pattern %CODE% and the code variable
    # - always from this function, call the outside function passing it the new source_template 
    #   as a string, the pattern %ALTCODE% and the altcode variable
    # - build this function taking the source_template, the pattern and the new value as arguments
    # - this function would find the beginning and the end of the pattern in the source_template, 
    #   and replace the pattern by the value passed in and return the new source_template
    # - from this function return the last source_tempalte alterated  
# ---------------------------------------------------------------------------------------------------------------------------------------------------#
    # also if we are only looking at passing the spec we do not need to convert req_id or template 
    # into a string data type as it is tested in a string context. 
    # another reason for doing this would be if we have validations when calling this method, 
    # ensuring we are only passing strings to it. 

    # altcode = code[0..7].insert(5, "-")
    # source_template.gsub(/[%]\bCODE\b[%]|[%]\bALTCODE\b[%]/, 
    #   '%CODE%' => req_id, 
    #   '%ALTCODE%' => altcode
    # )

  end
end
```

Happy coding!

