# Examples

## From the nools doc

### Flow

	define Message {
	  text : '',
	  constructor : function(message){
	    this.text = message;
	  }
	}

	//find any message that starts with hello
	rule Hello {
	  when {
	    m : Message m.text =~ /^hello(\s*world)?$/;
	  }
	  then {
	    modify(m, function(){this.text += " goodbye";});
	  }
	}

	//find all messages then end in goodbye
	rule Goodbye {
	  when {
	    m : Message m.text =~ /.*goodbye$/;
	  }
	  then {
	    console.log(m.text);
	  }
	}

### Test script

	[
	  {
	    "type":"defined",
	    "values": [
	      "Message"
	    ]
	  },
	  {
	    "type":"given",
	    "values": [
	       "new Message('hello')",
	       "new Message('hello world')",
	       "new Message('goodbye')"
	    ]
	  },
	  {
	    "type":"expect_facts",
	    "values": [
	      { "text": "hello goodbye" },
	      { "text": "hello world goodbye" },
	      { "text": "goodbye" }
	    ]
	  }
	]
