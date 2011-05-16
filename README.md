natural
=======

General natural language facilities for nodejs. Stemming, classification and
phonetics are currently supported.

At the moment most algorithms are english specific but long term some diversity
is in order.

Installation
------------

If you're just looking to consume natural without your own node application
please install the NPM

    npm install natural
    
If you're interested in contributing or hacking natural then this repo is the
place to start.

Stemmers
--------

Currently stemming is supported via the Porter and Lancaster (Paice/Husk)
algorithms.

    var natural = require('natural');
    
    // this example uses a porter stemmer.
    console.log(natural.PorterStemmer.stem("words")); // stem a single word
    
    // attach() patches stem() and tokenizeAndStem() to String as a shortcut to 
    // PorterStemmer.stem(token)
    natural.PorterStemmer.attach();
    console.log("i am waking up to the sounds of chainsaws".tokenizeAndStem());
    console.log("chainsaws".stem());

    // same thing can be done with a lancaster stemmer
    natural.LancasterStemmer.attach();
    console.log("i am waking up to the sounds of chainsaws".tokenizeAndStem());
    console.log("chainsaws".stem());


Bayes Naive Classifier
----------------------

    var natural = require('natural'), 
    	classifier = new natural.BayesClassifier();

    // you can train the classifier on sample text. it will use reasonable
    // defaults to tokenize and stem the text.
    classifier.train([{classification: 'buy', text: "i am long qqqq"},
                  {classification: 'buy', text: "buy the q's"},
                  {classification: 'sell', text: "short gold"},
                  {classification: 'sell', text: "sell gold"}
    ]);

    // sell
    console.log(classifier.classify('i am short silver'));

    // buy
    console.log(classifier.classify('i am long copper'));

    classifier = new natural.BayesClassifier();

    // the classifier can also be trained on and classify arrays of tokens,
    // strings, or any mixture. arrays let you use entirely custom data with 
    // your own tokenization/stemming if any at all.
    classifier.train([{classification: 'hockey', text: ['puck', 'shoot']},
                  {classification: 'hockey', text: 'goalies stop pucks.'},
                  {classification: 'stocks', text: ['stop', 'loss']},
                  {classification: 'stocks', text: 'creat a stop order'}
                  ]);

    console.log(classifier.classify('stop out at $100'));
    console.log(classifier.classify('stop the puck, fool!'));
    
    console.log(classifier.classify(['stop', 'out']));
    console.log(classifier.classify(['stop', 'puck', 'fool']));    

Metaphone Phonetics
-------------------

    var natural = require('natural'), metaphone = natural.Metaphone;

    var wordA = 'phonetics';
    var wordB = 'fonetix';

    // test the two words to see if they sound alike
    if(metaphone.compare(wordA, wordB))
        console.log('they sound alike!');
        
    // the raw phonetics are obtained with process()
    console.log(metaphone.process('phonetics'));

    // attaching will patch string with useful methods
    metaphone.attach();

    // soundsLike is essentially a shortcut to Metaphone.compare
    if(wordA.soundsLike(wordB))
        console.log('they sound alike!');
        
    // the raw phonetics are obtained with phonetics()
        console.log('phonetics'.phonetics());   

Copyright
---------

Copyright (c) 2011 Chris Umbel
