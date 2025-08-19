# Overview

This Erlang application implements a **Natural Language Processing Web Server** that performs various text analysis tasks including tokenization, sentiment analysis, and word frequency analysis. The application demonstrates all core Erlang functional programming concepts while providing practical NLP functionality.

## Features

- **Text Tokenization**: Splits text into individual words and cleans them
- **Sentiment Analysis**: Analyzes text for positive, negative, or neutral sentiment
- **Word Frequency Analysis**: Counts occurrences of each word in the text
- **Word Filtering**: Filters words based on minimum length criteria
- **Text Capitalization**: Capitalizes words using functional transformations
- **Web Server**: Simple HTTP-like server for processing requests

## Erlang Features Demonstrated

[Software Demo Video](http://youtube.link.goes.here)

## Usage Instructions:

Compile: c(nlp_web_app).
Run Demo: nlp_web_app:demo().
Start Server: nlp_web_app:start_server().
Process Text: nlp_web_app:process_text("Your text here").
Stop Server: nlp_web_app:stop_server().

### Basic Requirements (All Implemented)

1. **Display output using format**: 
   - Custom `format/2` wrapper function used throughout
   - Extensive logging and output formatting

2. **Pattern Matching in function parameters**:
   - `tokenize/1` matches different input types (list vs binary)
   - `calculate_sentiment_score/3` matches different sentiment scenarios
   - `server_loop/1` matches different message types
   - `filter_empty_tokens/1` matches list head/tail patterns

3. **Recursion**:
   - `filter_empty_tokens/1` - recursive token filtering
   - `word_frequency/2` - recursive frequency counting
   - `count_occurrences/2` - recursive occurrence counting
   - `server_loop/1` - recursive server message processing

4. **Guards**:
   - `tokenize/1` uses `when is_list(Text)` and `when is_binary(Text)`
   - `calculate_sentiment_score/3` uses `when Pos > Neg` conditions
   - `filter_empty_tokens/1` uses `when length(H) > 0`
   - `filter_words/2` uses `when MinLength >= 0`
   - `capitalize_word/1` uses character range guards

5. **Lists**:
   - Extensive list processing throughout all functions
   - List comprehensions and transformations
   - Head/tail pattern matching `[H|T]`

6. **Lambda functions with lists:map, lists:filter, lists:foldl**:
   - `lists:foldl/3` for sentiment counting
   - `lists:filter/2` for word filtering
   - `lists:map/2` for word capitalization and case conversion

### Additional Requirement (One Implemented)

**Process creation and message passing**:
- `start_server/1` spawns server process
- `server_loop/1` handles various message types
- Process registration with `register/2`
- Message passing with `!` operator
- **Exception handling** also implemented as bonus

## Installation and Setup

1. **Install Erlang**: Ensure Erlang is installed and `erl` is in your PATH
2. **Development Environment**: 
   - Visual Studio Code with Erlang extension (recommended)
   - Or use any text editor with Erlang shell

## Usage

### Compilation
```erlang
% In Erlang shell (erl)
1> c(nlp_web_app).
{ok,nlp_web_app}
```

### Running the Demo
```erlang
2> nlp_web_app:demo().
=== Natural Language Processing Demo ===
Original text: This is a great and amazing application! However, some parts are terrible and awful.
...
```

### Individual Function Usage

#### Text Tokenization
```erlang
3> nlp_web_app:tokenize("Hello world! This is great.").
["hello","world","this","is","great"]
```

#### Sentiment Analysis
```erlang
4> nlp_web_app:analyze_sentiment("This is amazing and wonderful!").
{positive,0.5}
```

#### Complete Text Processing
```erlang
5> nlp_web_app:process_text("Hello beautiful world").
[{tokens,["hello","beautiful","world"]},
 {sentiment,{neutral,0.0}},
 {word_count,3},
 {word_frequency,[{"world",1},{"beautiful",1},{"hello",1}]},
 {long_words,["beautiful"]},
 {capitalized,["Hello","Beautiful","World"]}]
```

### Web Server Operations
```erlang
% Start server on port 8080
6> nlp_web_app:start_server().
Starting NLP Web Server on port 8080
Server started with PID: <0.87.0>
ok

% Stop server
7> nlp_web_app:stop_server().
Server stopped successfully
ok

% Start server on custom port
8> nlp_web_app:start_server(3000).
Starting NLP Web Server on port 3000
...
```

## Function Reference

### Core NLP Functions
- `tokenize/1` - Tokenizes input text into word list
- `analyze_sentiment/1` - Analyzes text sentiment (positive/negative/neutral)
- `process_text/1` - Performs complete text analysis
- `word_frequency/1` - Calculates word frequency distribution
- `filter_words/2` - Filters words by minimum length
- `capitalize_words/1` - Capitalizes all words in list

### Server Functions
- `start_server/0` - Starts server on default port 8080
- `start_server/1` - Starts server on specified port
- `stop_server/0` - Stops the running server

### Utility Functions
- `format/2` - Wrapper for io:format with consistent output
- `demo/0` - Comprehensive demonstration of all features

## Example Output

```
=== Natural Language Processing Demo ===
Original text: This is a great and amazing application! However, some parts are terrible and awful.

--- Tokenization ---
Tokenizing text: This is a great and amazing application! However, some parts are terrible and awful.
Tokens: ["this","is","a","great","and","amazing","application","however","some","parts","are","terrible","and","awful"]

--- Sentiment Analysis ---
Analyzing sentiment for: This is a great and amazing application! However, some parts are terrible and awful.
Sentiment: {neutral,0.0}

--- Word Frequency ---
Word frequencies: [{"awful",1},{"terrible",1},{"are",1},{"parts",1},{"some",1},{"however",1},{"application",1},{"amazing",1},{"and",2},{"great",1},{"a",1},{"is",1},{"this",1}]

--- Filtered Words (length >= 5) ---
Long words: ["amazing","application","however","terrible"]

--- Capitalized Words ---
Capitalized: ["This","Is","A","Great","And","Amazing","Application","However","Some","Parts","Are","Terrible","And","Awful"]
```

## Technical Notes

### Error Handling
The application includes comprehensive error handling:
- Exception throwing for invalid inputs
- Try-catch blocks for server operations
- Input validation with guards

### Functional Programming Principles
- **Immutability**: All data structures are immutable
- **Pure Functions**: Functions produce outputs from inputs without side effects
- **Higher-Order Functions**: Extensive use of map, filter, and fold operations
- **Pattern Matching**: Comprehensive pattern matching for control flow

### Performance Considerations
- Tail-recursive functions where possible
- Efficient list processing
- Minimal memory allocation through list reuse

## Learning Objectives Met

This application demonstrates mastery of:
1. Functional programming paradigms
2. Erlang syntax and semantics
3. Process-oriented programming
4. Pattern matching and guards
5. Recursive programming techniques
6. List processing and higher-order functions
7. Basic concurrent programming concepts
8. Natural language processing fundamentals

## Extensions and Future Work

Potential enhancements:
- Add HTTP server functionality for real web requests
- Implement more sophisticated NLP algorithms
- Add support for multiple languages
- Create web interface for easier interaction
- Add persistent storage for text analysis history