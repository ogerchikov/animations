<details> 
<summary></summary>

digraph G {
  node [fontsize=8];
  // States
  "Idle" [label="Idle\nST = null\nHT = null\nTask = none\n=======\ncurrent_time=unresolved\nplay_state=idle"]
    subgraph cluster_0 {
		style=filled;
		color=lightgrey;
		node [style=filled,color=white];
        "Play-pending (HT, TL Inactive)" [label="Play-pending HT\nST = null\nHT = resolved\nTask = play\n=======\ncurrent_time=resolved\nplay_state=running", color=yellow]
        "Play-pending (ST, TL Inactive)" [label="Play-pending ST\nST = resolved\nHT = null\nTask = play\n=======\ncurrent_time=null\nplay_state=running", color=lightyellow1]
        "Pause-pending (HT, TL Inactive)" [label="Pause-pending HT\nST = null\nHT = resolved\nTask = pause\n=======\ncurrent_time=resolved\nplay_state=paused", color=lightskyblue1]
        "Pause-pending (ST, TL Inactive)" [label="Pause-pending ST\nST = resolved\nHT = null\nTask = pause\n=======\ncurrent_time=null\nplay_state=paused", color=aquamarine2]
        "Running (TL Inactive)" [label="Running\nST = resolved\nHT = null\nTask = none\n=======\ncurrent_time=null\nplay_state=running", color=lemonchiffon2]
        "Paused (TL Inactive)" [label="Paused\nST = null\nHT = resolved\nTask = none\n=======\ncurrent_time=resolved\nplay_state=paused", color=gold1]		
		label = "Timeline Inactive";
	}

	subgraph cluster_1 {
		node [style=filled fontsize=8];		
		label = "Timeline Active";
		color=blue
        "Play-pending (HT, TL Active)" [label="Play-pending HT\nST = null\nHT = resolved\nTask = play\n=======\ncurrent_time=resolved\nplay_state=running", color=yellow]
        "Play-pending (ST, TL Active)" [label="Play-pending ST\nST = resolved\nHT = null\nTask = play\n=======\ncurrent_time=resolved\nplay_state=running", color=lightyellow1]
        "Pause-pending (HT, TL Active)" [label="Pause-pending HT\nST = null\nHT = resolved\nTask = pause\n=======\ncurrent_time=resolved\nplay_state=paused", color=lightskyblue1]
        "Pause-pending (ST, TL Active)" [label="Pause-pending ST\nST = resolved\nHT = null\nTask = pause\n=======\ncurrent_time=resolved\nplay_state=paused", color=aquamarine2]
        "Running (TL Active)" [label="Running\nST = resolved\nHT = null\nTask = none\n=======\ncurrent_time=resolved\nplay_state=running", color=lemonchiffon2]
        "Paused (TL Active)" [label="Paused\nST = null\nHT = resolved\nTask = none\n=======\ncurrent_time=resolved\nplay_state=paused", color=gold1]

	}
  
  
  // Idle state
  "Idle" -> "Play-pending (HT, TL Active)" [ label="play()" color="red" ]
  "Idle" -> "Pause-pending (HT, TL Active)" [ label="pause()" color="green" ]
  "Idle" -> "Play-pending (ST, TL Inactive)" [ label="play()" color="red" ]
  "Idle" -> "Pause-pending (ST, TL Inactive)" [ label="pause()" color="green" ]

  // Play-pending (HT, TL Active) state
  "Play-pending (HT, TL Active)" -> "Running (TL Active)" [ label="ready" style=dashed ]
  "Play-pending (HT, TL Active)" -> "Play-pending (HT, TL Active)" [ label="play()" color="red" ]
  "Play-pending (HT, TL Active)" -> "Pause-pending (HT, TL Active)" [ label="pause()" color="green" ]
  "Play-pending (HT, TL Active)" -> "Play-pending (HT, TL Inactive)" [ label="Inactive" color="gray" ]

  // Play-pending state (ST, TL Active)
  "Play-pending (ST, TL Active)" -> "Running (TL Active)" [ label="ready" style=dashed ]
  "Play-pending (ST, TL Active)" -> "Play-pending (ST, TL Active)" [ label="play()" color="red" ]
  "Play-pending (ST, TL Active)" -> "Pause-pending (ST, TL Active)" [ label="pause()" color="green" ]
  "Play-pending (ST, TL Active)" -> "Play-pending (ST, TL Inactive)" [ label="Inactive" color="gray" ]

  // Pause-pending (HT, TL Active) state
  "Pause-pending (HT, TL Active)" -> "Paused (TL Active)" [ label="ready" style=dashed ]
  "Pause-pending (HT, TL Active)" -> "Play-pending (HT, TL Active)" [ label="play()" color="red" ]
  "Pause-pending (HT, TL Active)" -> "Pause-pending (HT, TL Active)" [ label="pause()" color="green" ]
  "Pause-pending (HT, TL Active)" -> "Pause-pending (HT, TL Inactive)" [ label="Inactive" color="gray" ]

  // Pause-pending (ST, TL Active) state
  "Pause-pending (ST, TL Active)" -> "Paused (TL Active)" [ label="ready" style=dashed ]
  // (Following is the aborted paused behavior)
  "Pause-pending (ST, TL Active)" -> "Play-pending (ST, TL Active)" [ label="play()" color="red" ]
  "Pause-pending (ST, TL Active)" -> "Pause-pending (ST, TL Active)" [ label="pause()" color="green" ]
  "Pause-pending (ST, TL Active)" -> "Pause-pending (ST, TL Inactive)" [ label="Inactive" color="gray" ]

  // Running state
  "Running (TL Active)" -> "Running (TL Active)" [ label="play()" color="red" ]
  "Running (TL Active)" -> "Pause-pending (ST, TL Active)" [ label="pause()" color="green" ]
  "Running (TL Active)" -> "Running (TL Inactive)" [ label="Inactive" color="gray" ]

  // Paused state
  "Paused (TL Active)" -> "Paused (TL Active)" [ label="pause()" color="green" ]
  "Paused (TL Active)" -> "Play-pending (HT, TL Active)" [ label="play()" color="red" ]
  "Paused (TL Active)" -> "Paused (TL Inactive)" [ label="Inactive" color="gray" ]
  
  //Play-pending (HT, TL Inactive) state
  "Play-pending (HT, TL Inactive)" -> "Play-pending (HT, TL Inactive)" [ label="play()" color="red" ]
  "Play-pending (HT, TL Inactive)" -> "Pause-pending (HT, TL Inactive)" [ label="pause()" color="green" ]
  "Play-pending (HT, TL Inactive)" -> "Play-pending (HT, TL Active)" [ label="Active" color="black" ]
  
  //Play-pending (ST, TL Inactive)
  "Play-pending (ST, TL Inactive)" -> "Play-pending (ST, TL Inactive)" [ label="play()" color="red" ]
  "Play-pending (ST, TL Inactive)" -> "Pause-pending (ST, TL Inactive)" [ label="pause()" color="green" ]
  "Play-pending (ST, TL Inactive)" -> "Play-pending (ST, TL Active)" [ label="Active" color="black" ]
  
  //Pause-pending (HT, TL Inactive)
  "Pause-pending (HT, TL Inactive)" -> "Play-pending (HT, TL Inactive)" [ label="play()" color="red" ]
  "Pause-pending (HT, TL Inactive)" -> "Pause-pending (HT, TL Inactive)" [ label="pause()" color="green" ]
  "Pause-pending (HT, TL Inactive)" -> "Pause-pending (HT, TL Active)" [ label="Active" color="black" ]
  
  //Pause-pending (ST, TL Inactive)
  "Pause-pending (ST, TL Inactive)" -> "Play-pending (ST, TL Inactive)" [ label="play()" color="red" ]
  "Pause-pending (ST, TL Inactive)" -> "Pause-pending (ST, TL Inactive)" [ label="pause()" color="green" ]
  "Pause-pending (ST, TL Inactive)" -> "Pause-pending (ST, TL Active)" [ label="Active" color="black" ]
  
  //TL Inactive
  "Running (TL Inactive)" -> "Running (TL Inactive)" [ label="play()" color="red" ]
  "Running (TL Inactive)" -> "Pause-pending (ST, TL Inactive)" [ label="pause()" color="green" ]
  "Running (TL Inactive)" -> "Running (TL Active)" [ label="Active" color="black" ]
  
  //Paused (TL Inactive)
  "Paused (TL Inactive)" -> "Paused (TL Inactive)" [ label="pause()" color="green" ]
  "Paused (TL Inactive)" -> "Play-pending (HT, TL Inactive)" [ label="play()" color="red" ]
  "Paused (TL Inactive)" -> "Paused (TL Active)" [ label="Active" color="black" ]
}
</details>
