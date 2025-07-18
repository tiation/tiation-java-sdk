# Tiation Java SDK

<div align="center">
  <img src="https://via.placeholder.com/800x200/1a1a2e/16213e?text=Tiation+Java+SDK" alt="Tiation Java SDK Banner" />
  
  <h3>Official Java SDK for Tiation APIs</h3>
  
  [![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](https://github.com/tiation/tiation-java-sdk)
  [![Java](https://img.shields.io/badge/java-11+-brightgreen.svg)](https://openjdk.org)
  [![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
  [![Enterprise](https://img.shields.io/badge/enterprise-ready-purple.svg)](https://github.com/tiation)
</div>

## 🌟 Overview

The Tiation Java SDK provides a robust, enterprise-grade interface for interacting with all Tiation services. Built with modern Java practices and designed for enterprise applications, it offers seamless integration with your Java/JVM applications.

## ✨ Features

- **🚀 Enterprise-Ready**: Built for large-scale Java applications
- **🔒 Enterprise Security**: Built-in authentication and rate limiting
- **📊 Real-time Analytics**: Live metrics and business intelligence
- **🔄 Reactive Support**: Full support for reactive programming with RxJava
- **🛡️ Type Safety**: Strongly typed interfaces and comprehensive error handling
- **📚 Rich Documentation**: Comprehensive JavaDoc and examples
- **⚡ High Performance**: Optimized for enterprise workloads

## 📦 Installation

### Maven
```xml
<dependency>
    <groupId>com.tiation</groupId>
    <artifactId>tiation-java-sdk</artifactId>
    <version>1.0.0</version>
</dependency>
```

### Gradle
```gradle
implementation 'com.tiation:tiation-java-sdk:1.0.0'
```

## 🚀 Quick Start

```java
import com.tiation.TiationClient;
import com.tiation.models.Analytics;
import com.tiation.models.Workflow;

public class Example {
    public static void main(String[] args) {
        // Initialize client
        TiationClient client = new TiationClient("your_api_key");
        
        // Get business analytics
        Analytics analytics = client.analytics().getMetrics();
        System.out.printf("Total revenue: $%.2f%n", analytics.getRevenue());
        
        // Create automation workflow
        Workflow workflow = new Workflow.Builder()
            .name("Customer Onboarding")
            .trigger("user_signup")
            .actions(List.of("send_welcome_email", "create_profile"))
            .build();
            
        Workflow result = client.automation().createWorkflow(workflow);
        System.out.printf("Workflow created: %s%n", result.getId());
    }
}
```

## 📚 Documentation

### Authentication

```java
import com.tiation.TiationClient;
import com.tiation.TiationConfig;

// API Key authentication
TiationClient client = new TiationClient("your_api_key");

// OAuth2 authentication
TiationConfig config = new TiationConfig.Builder()
    .clientId("your_client_id")
    .clientSecret("your_client_secret")
    .oauthToken("your_oauth_token")
    .build();
    
TiationClient client = new TiationClient(config);
```

### Core Services

#### Analytics & Metrics
```java
import com.tiation.models.MetricsRequest;
import com.tiation.models.Dashboard;

// Get business metrics
MetricsRequest request = new MetricsRequest.Builder()
    .startDate("2024-01-01")
    .endDate("2024-12-31")
    .build();
    
Analytics metrics = client.analytics().getMetrics(request);

// Real-time dashboard data
Dashboard dashboard = client.analytics().getDashboard();
```

#### Automation Workflows
```java
import com.tiation.models.ExecuteRequest;
import java.util.Map;

// List workflows
List<Workflow> workflows = client.automation().listWorkflows();

// Execute workflow
ExecuteRequest request = new ExecuteRequest.Builder()
    .workflowId("workflow_123")
    .parameters(Map.of("user_id", "user_456"))
    .build();
    
ExecuteResult result = client.automation().executeWorkflow(request);
```

#### Content Management
```java
import com.tiation.models.Content;
import com.tiation.models.ContentUpdate;

// Create content
Content content = new Content.Builder()
    .title("New Product Launch")
    .body("Exciting new features available now!")
    .category("announcements")
    .build();
    
Content result = client.cms().createContent(content);

// Update content
ContentUpdate update = new ContentUpdate.Builder()
    .status("published")
    .build();
    
client.cms().updateContent(result.getId(), update);
```

### Advanced Features

#### Reactive Programming
```java
import com.tiation.reactive.ReactiveTiationClient;
import io.reactivex.rxjava3.core.Observable;

ReactiveTiationClient reactiveClient = new ReactiveTiationClient("your_api_key");

// Reactive API calls
Observable<Analytics> metricsObservable = reactiveClient.analytics().getMetrics();
Observable<List<Workflow>> workflowsObservable = reactiveClient.automation().listWorkflows();

// Combine observables
Observable.zip(
    metricsObservable,
    workflowsObservable,
    (metrics, workflows) -> new CombinedResult(metrics, workflows)
).subscribe(result -> {
    // Process combined result
});
```

#### Error Handling
```java
import com.tiation.exceptions.TiationException;
import com.tiation.exceptions.RateLimitException;
import com.tiation.exceptions.ApiException;

try {
    Analytics analytics = client.analytics().getMetrics();
} catch (RateLimitException e) {
    System.out.printf("Rate limit exceeded, retry after: %d seconds%n", e.getRetryAfter());
} catch (ApiException e) {
    System.out.printf("API error: %s (code: %d)%n", e.getMessage(), e.getCode());
} catch (TiationException e) {
    System.out.printf("SDK error: %s%n", e.getMessage());
}
```

## 🔧 Configuration

### Properties File
```properties
# application.properties
tiation.api.key=your_api_key
tiation.base.url=https://api.tiation.com
tiation.timeout=30
tiation.max.retries=3
tiation.retry.delay=1000
```

### Configuration Class
```java
TiationConfig config = new TiationConfig.Builder()
    .apiKey("your_api_key")
    .baseUrl("https://api.tiation.com")
    .timeout(Duration.ofSeconds(30))
    .maxRetries(3)
    .retryDelay(Duration.ofSeconds(1))
    .build();

TiationClient client = new TiationClient(config);
```

## 🏢 Enterprise Features

### Batch Operations
```java
import com.tiation.models.BatchRequest;

// Batch create content
List<Content> contents = List.of(
    new Content.Builder().title("Article 1").body("Content 1").build(),
    new Content.Builder().title("Article 2").body("Content 2").build()
);

BatchResult<Content> results = client.cms().batchCreate(contents);

// Batch analytics
List<MetricsRequest> requests = List.of(
    new MetricsRequest.Builder().metric("revenue").period("monthly").build(),
    new MetricsRequest.Builder().metric("users").period("daily").build()
);

BatchResult<Analytics> metrics = client.analytics().batchMetrics(requests);
```

### Spring Boot Integration
```java
@Configuration
@EnableTiation
public class TiationConfig {
    
    @Bean
    public TiationClient tiationClient() {
        return new TiationClient("your_api_key");
    }
}

@Service
public class BusinessService {
    
    @Autowired
    private TiationClient tiationClient;
    
    public void processBusinessLogic() {
        Analytics analytics = tiationClient.analytics().getMetrics();
        // Process analytics
    }
}
```

### Webhook Integration
```java
import com.tiation.webhooks.WebhookProcessor;
import com.tiation.webhooks.WebhookEvent;

WebhookProcessor processor = new WebhookProcessor("your_webhook_secret");

processor.handleEvent("user.created", event -> {
    System.out.printf("New user: %s%n", event.getData().get("email"));
    
    // Trigger welcome workflow
    ExecuteRequest request = new ExecuteRequest.Builder()
        .workflowId("welcome_workflow")
        .parameters(Map.of("user_id", event.getData().get("id")))
        .build();
        
    return tiationClient.automation().executeWorkflow(request);
});
```

## 🧪 Testing

```bash
# Run tests
./mvnw test

# Run with coverage
./mvnw test jacoco:report

# Run specific test
./mvnw test -Dtest=AnalyticsServiceTest#testGetMetrics
```

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

### Development Setup

```bash
# Clone repository
git clone https://github.com/tiation/tiation-java-sdk.git
cd tiation-java-sdk

# Build project
./mvnw clean compile

# Run tests
./mvnw test
```

## 📞 Support

- **Documentation**: [Java SDK Docs](https://docs.tiation.com/java-sdk)
- **JavaDoc**: [API Reference](https://javadoc.tiation.com/java-sdk)
- **Issues**: [GitHub Issues](https://github.com/tiation/tiation-java-sdk/issues)
- **Enterprise Support**: [tiatheone@protonmail.com](mailto:tiatheone@protonmail.com)

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

<div align="center">
  <p>Built with ❤️ by <a href="https://github.com/tiation">Tiation</a></p>
  <p>Enterprise-grade Java SDK for mission-critical applications</p>
</div>
