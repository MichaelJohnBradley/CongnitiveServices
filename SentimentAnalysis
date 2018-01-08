using System;
using System.Collections.Generic;
using System.Configuration;
using System.Threading.Tasks;
using Microsoft.Azure.CognitiveServices.Language.TextAnalytics;
using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;

    public class SentimentAnalysis
    {
        private readonly ITextAnalyticsAPI _client;
        public SentimentAnalysis()
        {
            var regionName = ConfigurationManager.AppSettings["TextAnalyticsApiRegion"];
            var subscriptionKey = ConfigurationManager.AppSettings["TextAnalyticsApiKey"];

            var validRegion = Enum.TryParse(regionName, out AzureRegions region);
            if (string.IsNullOrEmpty(subscriptionKey) || !validRegion)                
            {
                throw new Exception("Unable to create Sentiment Analysis Client - check the region and subscription key");
            }

            var client = new TextAnalyticsAPI
            {
                AzureRegion = region,
                SubscriptionKey = subscriptionKey
            };
            _client = client;
        }

        public async Task<SentimentBatchResult> GetSentimentAsync(List<SentimentPhrase> sentimentPhrases)
        {
            if (sentimentPhrases.Count > 1000)
                throw new Exception("The limit of 1000 phrases has been exceeded - none of your list will have been sent for analysis.");

            var items = new List<MultiLanguageInput>();

            foreach (var s in sentimentPhrases)
            {
                var input = new MultiLanguageInput(s.LanguageCode, s.Id,s.Phrase);
                items.Add(input);
            }

            var result = _client.SentimentAsync(new MultiLanguageBatchInput(items));
            return await result;
        }

        public SentimentBatchResult GetSentiment(List<SentimentPhrase> sentimentPhrases)
        {
            if (sentimentPhrases.Count > 1000)
                throw new Exception("The limit of 1000 phrases has been exceeded - none of your list will have been sent for analysis ");
            var items = new List<MultiLanguageInput>();

            foreach (var s in sentimentPhrases)
            {
                var input = new MultiLanguageInput(s.LanguageCode, s.Id, s.Phrase);
                items.Add(input);
            }

            var result = _client.Sentiment(new MultiLanguageBatchInput(items));
            return result;
        }

        public class SentimentPhrase
        {
            public string Id { get; set; }
            public string Phrase { get; set; }
            public string LanguageCode { get; set; }
        }
    }
