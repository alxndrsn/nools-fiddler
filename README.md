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

## Pregnancy followups

### Flow

	define Pregnancy {
	  start : '',
	  constructor : function() {
	    this.start = Nootils.date();
	  }
	}

	define Checkup {
	  due : '',
	  constructor : function(due) {
	    this.due = due;
	  }
	}

	rule TrimesterCheckup {
	  when {
	    p : Pregnancy
	  }
	  then {
	    emit('checkup', new Checkup(Nootils.datePlus(p.start, 90)))
	  }
	}

### Test script

	[
	  {
	    "type":"defined",
	    "values": [ "Pregnancy", "Checkup" ]
	  },
	  {
	    "type":"fast_forward",
	    "days":1
	  },
	  {
	    "type":"date",
	    "value":0
	  },
	  {
	    "type":"given",
	    "values": [
	       "new Pregnancy()"
	    ]
	  },
	  {
	    "type":"expect_emits",
	    "event": "checkup",
	    "values": [
	      [ { "due":"1970-04-01" } ]
	    ]
	  }
	]
