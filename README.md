================================================================
  PHISHING URL DETECTION SYSTEM
  Using Finite Automata and Regular Expressions
================================================================
  Student    : Najia Nayab
  Reg. No.   : NUM-BSCS-2024-62
  Course     : Theory of Automata
  Instructor : Mr. Muhammad Bilal
  University : Namal University, Mianwali
================================================================


PROJECT STRUCTURE
─────────────────
  dfa_engine.py         Core DFA — states, transitions, logging
  pattern_matcher.py    Regex pattern library (keyword/domain/symbol)
  detector.py           Main controller — wires DFA + regex together
  gui.py                PyQt6 graphical interface
  demo.py               Step-by-step terminal demonstration
  visualizer.py         Graphviz DFA diagram export
  batch_scanner.py      Scan all URLs from a dataset file
  report_generator.py   Save full scan report to a .txt file
  url_dataset.txt       40 labeled test URLs (20 phishing, 20 safe)
  README.txt            This file


REQUIREMENTS
────────────
  Python 3.8 or higher

  Python libraries:
      pip install PyQt6 graphviz

  System Graphviz (for diagram export):
      Windows  →  winget install graphviz
      Mac      →  brew install graphviz
      Linux    →  sudo apt install graphviz

  All files must be in the same folder.


HOW TO RUN
──────────

1. GUI (main application)
   ─────────────────────────────────────────────────────────────
   python gui.py

   - Enter any URL in the input bar and press Scan or Enter
   - Click Demo URLs to load sample URLs automatically
   - Watch the DFA state machine update in real time
   - Terminal log shows every regex match and state transition


2. Terminal Demo (for presentation to instructor)
   ─────────────────────────────────────────────────────────────
   python demo.py

   Walks through 4 parts step by step (press Enter to advance):
     Part 1 — shows all active regex patterns by category
     Part 2 — formal DFA definition: Q, Σ, δ, q0, F
     Part 3 — scans 7 demo URLs with full trace
     Part 4 — accuracy summary


3. DFA Diagram Export
   ─────────────────────────────────────────────────────────────
   python visualizer.py
   Saves: dfa_diagram.png

   python visualizer.py "http://paypal-secure-login.xyz/verify"
   Saves: dfa_diagram.png + dfa_diagram_scan.png
          (scan diagram highlights the active path in colour)


4. Batch Scanner
   ─────────────────────────────────────────────────────────────
   python batch_scanner.py
   python batch_scanner.py my_urls.txt   (custom file)

   Reads url_dataset.txt, scans all 40 URLs, prints a table
   showing expected label, verdict, threat score, and accuracy.


5. Report Generator
   ─────────────────────────────────────────────────────────────
   python report_generator.py

   Runs full batch scan and saves a detailed .txt report file
   named:  scan_report_YYYY-MM-DD_HH-MM-SS.txt

   Report includes:
     - System configuration
     - Per-URL verdict, DFA path, regex matches
     - Overall accuracy and misclassified URLs


DFA DESIGN
──────────
  States:   Q  = { q0, q1, q2, q3, q4 }
  Alphabet: Σ  = { keyword, domain, symbol, clean }
  Start:    q0 = Start State
  Accept:   F  = { q3 (Phishing), q4 (Safe) }

  Transition table δ(state, event):
  ┌──────┬─────────┬────────┬────────┬───────┐
  │State │ keyword │ domain │ symbol │ clean │
  ├──────┼─────────┼────────┼────────┼───────┤
  │ q0   │   q1    │  q2    │  q1    │  q4   │
  │ q1   │   q1    │  q2    │  q2    │  q1   │
  │ q2   │   q3    │  q3    │  q3    │  q2   │
  │ q3   │   q3    │  q3    │  q3    │  q3   │
  │ q4   │   q4    │  q4    │  q4    │  q4   │
  └──────┴─────────┴────────┴────────┴───────┘


REGEX PATTERN CATEGORIES
─────────────────────────
  Keywords  →  login, verify, secure, banking, update, account,
               confirm, password, credential, signin, wallet
  Domains   →  .xyz .ru .tk .ml .ga .cf .gq
               -secure- -login -verify amazon- paypal. apple-
  Symbols   →  @  %xx (URL encoding)  //  (double slash)


THREAT SCORING
──────────────
  Each keyword match  +15 points
  Each domain match   +25 points
  Each symbol match   +20 points
  Maximum score       100 points
  Phishing threshold  score >= 50 OR final state == q3


================================================================
  End of README
================================================================
