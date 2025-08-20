% Natural Language Processing Web Application
% This module demonstrates all required Erlang features while providing NLP functionality

-module(nlp_web_app).
-export([start_server/0, start_server/1, stop_server/0]).
-export([tokenize/1, analyze_sentiment/1, process_text/1]).
-export([word_frequency/1, filter_words/2, capitalize_words/1]).

% Start the web server (default port 8080)
start_server() ->
    start_server(8080).

% Start the web server on specified port
% Demonstrates: Process creation and message passing
start_server(Port) ->
    format("Starting NLP Web Server on port ~w~n", [Port]),
    ServerPid = spawn(fun() -> server_loop(Port) end),
    register(nlp_server, ServerPid),
    ServerPid ! {start, Port},
    format("Server started with PID: ~w~n", [ServerPid]),
    ok.

% Stop the web server
% Demonstrates: Exception handling
stop_server() ->
    try
        nlp_server ! stop,
        format("Server stopped successfully~n", [])
    catch
        error:badarg ->
            format("Error: Server not running~n", []);
        _:Error ->
            format("Unexpected error stopping server: ~w~n", [Error])
    end.

% Main server loop
% Demonstrates: Pattern matching, recursion, case of
server_loop(Port) ->
    receive
        {start, Port} ->
            format("Server listening on port ~w~n", [Port]),
            format("Available endpoints:~n", []),
            format("  POST /tokenize - Tokenize text~n", []),
            format("  POST /sentiment - Analyze sentiment~n", []),
            format("  POST /process - Complete text processing~n", []),
            server_loop(Port);
        
        {http_request, From, Method, Path, Body} ->
            Response = case {Method, Path} of
                {"POST", "/tokenize"} ->
                    Tokens = tokenize(Body),
                    format_json_response("tokens", Tokens);
                {"POST", "/sentiment"} ->
                    Sentiment = analyze_sentiment(Body),
                    format_json_response("sentiment", Sentiment);
                {"POST", "/process"} ->
                    Result = process_text(Body),
                    format_json_response("analysis", Result);
                _ ->
                    format_error_response("Unknown endpoint")
            end,
            From ! {http_response, Response},
            server_loop(Port);
        
        stop ->
            format("Server shutting down~n", []);
        
        Unknown ->
            format("Unknown message received: ~w~n", [Unknown]),
            server_loop(Port)
    end.

% Text tokenization function
% Demonstrates: Pattern matching, recursion, guards
tokenize(Text) when is_list(Text) ->
    format("Tokenizing text: ~s~n", [Text]),
    Words = string:tokens(Text, " \t\n.,!?;:"),
    CleanWords = lists:map(fun string:to_lower/1, Words),
    filter_empty_tokens(CleanWords);

tokenize(Text) when is_binary(Text) ->
    tokenize(binary_to_list(Text));

tokenize(_) ->
    throw({error, invalid_input}).

% Filter empty tokens recursively
% Demonstrates: Pattern matching, recursion, guards
filter_empty_tokens([]) -> [];
filter_empty_tokens([H|T]) when length(H) > 0 ->
    [H | filter_empty_tokens(T)];
filter_empty_tokens([_|T]) ->
    filter_empty_tokens(T).

% Sentiment analysis function
% Demonstrates: Pattern matching, lists, lambda functions with lists:foldl
analyze_sentiment(Text) when is_list(Text) ->
    format("Analyzing sentiment for: ~s~n", [Text]),
    Tokens = tokenize(Text),
    
    % Positive and negative word lists
    PositiveWords = ["good", "great", "excellent", "amazing", "wonderful", 
                     "fantastic", "awesome", "brilliant", "outstanding", "perfect"],
    NegativeWords = ["bad", "terrible", "awful", "horrible", "disgusting",
                     "disappointing", "poor", "worst", "hate", "dislike"],
    
    % Count sentiment words using foldl
    {PositiveCount, NegativeCount} = lists:foldl(
        fun(Word, {Pos, Neg}) ->
            case lists:member(Word, PositiveWords) of
                true -> {Pos + 1, Neg};
                false ->
                    case lists:member(Word, NegativeWords) of
                        true -> {Pos, Neg + 1};
                        false -> {Pos, Neg}
                    end
            end
        end,
        {0, 0},
        Tokens
    ),
    
    % Determine overall sentiment
    calculate_sentiment_score(PositiveCount, NegativeCount, length(Tokens));

analyze_sentiment(Text) when is_binary(Text) ->
    analyze_sentiment(binary_to_list(Text)).

% Calculate sentiment score
% Demonstrates: Pattern matching, guards
calculate_sentiment_score(Pos, Neg, Total) when Pos > Neg ->
    Score = (Pos - Neg) / Total,
    {positive, Score};

calculate_sentiment_score(Pos, Neg, Total) when Neg > Pos ->
    Score = (Neg - Pos) / Total,
    {negative, Score};

calculate_sentiment_score(_, _, _) ->
    {neutral, 0.0}.

% Complete text processing
% Demonstrates: Pattern matching, multiple function calls
process_text(Text) ->
    format("Processing text: ~s~n", [Text]),
    Tokens = tokenize(Text),
    Sentiment = analyze_sentiment(Text),
    WordFreq = word_frequency(Tokens),
    LongWords = filter_words(Tokens, 5), % Words longer than 5 characters
    CapitalizedWords = capitalize_words(Tokens),
    
    [{tokens, Tokens},
     {sentiment, Sentiment},
     {word_count, length(Tokens)},
     {word_frequency, WordFreq},
     {long_words, LongWords},
     {capitalized, CapitalizedWords}].

% Calculate word frequency
% Demonstrates: Lists, recursion, pattern matching
word_frequency(Words) ->
    word_frequency(Words, []).

word_frequency([], Acc) -> Acc;
word_frequency([Word|Rest], Acc) ->
    Count = count_occurrences(Word, [Word|Rest]),
    FilteredRest = lists:filter(fun(W) -> W =/= Word end, Rest),
    word_frequency(FilteredRest, [{Word, Count}|Acc]).

% Count word occurrences
% Demonstrates: Recursion, guards, pattern matching
count_occurrences(_, []) -> 0;
count_occurrences(Word, [Word|T]) -> 1 + count_occurrences(Word, T);
count_occurrences(Word, [_|T]) -> count_occurrences(Word, T).

% Filter words by minimum length
% Demonstrates: Lambda functions with lists:filter, guards
filter_words(Words, MinLength) when MinLength >= 0 ->
    lists:filter(fun(Word) -> length(Word) >= MinLength end, Words);

filter_words(_, _) ->
    throw({error, invalid_min_length}).

% Capitalize words
% Demonstrates: Lambda functions with lists:map
capitalize_words(Words) ->
    lists:map(fun capitalize_word/1, Words).

% Capitalize a single word
% Demonstrates: Pattern matching, guards
capitalize_word([]) -> [];
capitalize_word([H|T]) when H >= $a, H =< $z ->
    [H - 32 | T];
capitalize_word(Word) -> Word.

% Format JSON-like response
format_json_response(Key, Value) ->
    format("~s: ~w~n", [Key, Value]),
    {ok, Key, Value}.

% Format error response
format_error_response(Message) ->
    format("Error: ~s~n", [Message]),
    {error, Message}.

% Wrapper for io:format to demonstrate format usage
format(Format, Args) ->
    io:format(Format, Args).

% Demo function to test all features
demo() ->
    format("=== Natural Language Processing Demo ===~n", []),
    
    % Test text
    TestText = "This is a great and amazing application! However, some parts are terrible and awful.",
    
    format("Original text: ~s~n", [TestText]),
    format("~n--- Tokenization ---~n", []),
    Tokens = tokenize(TestText),
    format("Tokens: ~w~n", [Tokens]),
    
    format("~n--- Sentiment Analysis ---~n", []),
    Sentiment = analyze_sentiment(TestText),
    format("Sentiment: ~w~n", [Sentiment]),
    
    format("~n--- Word Frequency ---~n", []),
    Frequency = word_frequency(Tokens),
    format("Word frequencies: ~w~n", [Frequency]),
    
    format("~n--- Filtered Words (length >= 5) ---~n", []),
    LongWords = filter_words(Tokens, 5),
    format("Long words: ~w~n", [LongWords]),
    
    format("~n--- Capitalized Words ---~n", []),
    CapWords = capitalize_words(Tokens),
    format("Capitalized: ~w~n", [CapWords]),
    
    format("~n--- Complete Processing ---~n", []),
    Result = process_text(TestText),
    format("Complete analysis: ~w~n", [Result]),
    
    format("~n--- Server Demo ---~n", []),
    start_server(8080),
    timer:sleep(1000),
    stop_server(),
    
    format("~n=== Demo Complete ===~n", []).

% Usage examples in comments:
% 1. Compile: c(nlp_web_app).
% 2. Run demo: nlp_web_app:demo().
% 3. Start server: nlp_web_app:start_server().
% 4. Process text: nlp_web_app:process_text("Your text here").
% 5. Stop server: nlp_web_app:stop_server().