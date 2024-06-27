---


---

<p>how do i do the call to an API url, for example <a href="https://toto/titi/rest/alerte/lire">https://toto/titi/rest/alerte/lire</a>, which returns a json, in my Java 8 Spring 5 project?</p>
<p>To make a call to an API URL in a Java 8 Spring 5 project, you can use the <code>RestTemplate</code> class provided by Spring. Here is a step-by-step guide on how to achieve this:</p>
<ol>
<li>
<p><strong>Add Dependencies:</strong><br>
Ensure you have the necessary dependencies in your <code>pom.xml</code> file for Spring Web and any other libraries you might need (like JSON processing).</p>
<pre class=" language-xml"><code class="prism  language-xml"><span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>dependencies</span><span class="token punctuation">&gt;</span></span>
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>dependency</span><span class="token punctuation">&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>groupId</span><span class="token punctuation">&gt;</span></span>org.springframework.boot<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>groupId</span><span class="token punctuation">&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>artifactId</span><span class="token punctuation">&gt;</span></span>spring-boot-starter-web<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>artifactId</span><span class="token punctuation">&gt;</span></span>
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>dependency</span><span class="token punctuation">&gt;</span></span>
    <span class="token comment">&lt;!-- Add other dependencies as needed --&gt;</span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>dependencies</span><span class="token punctuation">&gt;</span></span>
</code></pre>
</li>
<li>
<p><strong>Create a Configuration Class (Optional):</strong><br>
If you want to configure <code>RestTemplate</code> beans, you can create a configuration class.</p>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">import</span> org<span class="token punctuation">.</span>springframework<span class="token punctuation">.</span>context<span class="token punctuation">.</span>annotation<span class="token punctuation">.</span>Bean<span class="token punctuation">;</span>
<span class="token keyword">import</span> org<span class="token punctuation">.</span>springframework<span class="token punctuation">.</span>context<span class="token punctuation">.</span>annotation<span class="token punctuation">.</span>Configuration<span class="token punctuation">;</span>
<span class="token keyword">import</span> org<span class="token punctuation">.</span>springframework<span class="token punctuation">.</span>web<span class="token punctuation">.</span>client<span class="token punctuation">.</span>RestTemplate<span class="token punctuation">;</span>

<span class="token annotation punctuation">@Configuration</span>
<span class="token keyword">public</span> <span class="token keyword">class</span> <span class="token class-name">AppConfig</span> <span class="token punctuation">{</span>

    <span class="token annotation punctuation">@Bean</span>
    <span class="token keyword">public</span> RestTemplate <span class="token function">restTemplate</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">return</span> <span class="token keyword">new</span> <span class="token class-name">RestTemplate</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
</li>
<li>
<p><strong>Use <code>RestTemplate</code> to Make the API Call:</strong><br>
In your service class or wherever you need to make the API call, use the <code>RestTemplate</code> to call the API and handle the JSON response.</p>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">import</span> org<span class="token punctuation">.</span>springframework<span class="token punctuation">.</span>beans<span class="token punctuation">.</span>factory<span class="token punctuation">.</span>annotation<span class="token punctuation">.</span>Autowired<span class="token punctuation">;</span>
<span class="token keyword">import</span> org<span class="token punctuation">.</span>springframework<span class="token punctuation">.</span>stereotype<span class="token punctuation">.</span>Service<span class="token punctuation">;</span>
<span class="token keyword">import</span> org<span class="token punctuation">.</span>springframework<span class="token punctuation">.</span>web<span class="token punctuation">.</span>client<span class="token punctuation">.</span>RestTemplate<span class="token punctuation">;</span>
<span class="token keyword">import</span> org<span class="token punctuation">.</span>springframework<span class="token punctuation">.</span>http<span class="token punctuation">.</span>ResponseEntity<span class="token punctuation">;</span>

<span class="token annotation punctuation">@Service</span>
<span class="token keyword">public</span> <span class="token keyword">class</span> <span class="token class-name">ApiService</span> <span class="token punctuation">{</span>

    <span class="token annotation punctuation">@Autowired</span>
    <span class="token keyword">private</span> RestTemplate restTemplate<span class="token punctuation">;</span>

    <span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">callApi</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        String url <span class="token operator">=</span> <span class="token string">"https://toto/titi/rest/alerte/lire"</span><span class="token punctuation">;</span>

        <span class="token comment">// Making the API call and getting the response as a JSON string</span>
        ResponseEntity<span class="token operator">&lt;</span>String<span class="token operator">&gt;</span> response <span class="token operator">=</span> restTemplate<span class="token punctuation">.</span><span class="token function">getForEntity</span><span class="token punctuation">(</span>url<span class="token punctuation">,</span> String<span class="token punctuation">.</span><span class="token keyword">class</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

        <span class="token keyword">if</span> <span class="token punctuation">(</span>response<span class="token punctuation">.</span><span class="token function">getStatusCode</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">is2xxSuccessful</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
            String jsonResponse <span class="token operator">=</span> response<span class="token punctuation">.</span><span class="token function">getBody</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
            System<span class="token punctuation">.</span>out<span class="token punctuation">.</span><span class="token function">println</span><span class="token punctuation">(</span><span class="token string">"Response: "</span> <span class="token operator">+</span> jsonResponse<span class="token punctuation">)</span><span class="token punctuation">;</span>

            <span class="token comment">// If you need to map the JSON response to a Java object, you can use ObjectMapper</span>
            ObjectMapper objectMapper <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">ObjectMapper</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
            MyResponseObject responseObject <span class="token operator">=</span> objectMapper<span class="token punctuation">.</span><span class="token function">readValue</span><span class="token punctuation">(</span>jsonResponse<span class="token punctuation">,</span> MyResponseObject<span class="token punctuation">.</span><span class="token keyword">class</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
            System<span class="token punctuation">.</span>out<span class="token punctuation">.</span><span class="token function">println</span><span class="token punctuation">(</span><span class="token string">"Mapped Response: "</span> <span class="token operator">+</span> responseObject<span class="token punctuation">)</span><span class="token punctuation">;</span>
        <span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>
            System<span class="token punctuation">.</span>out<span class="token punctuation">.</span><span class="token function">println</span><span class="token punctuation">(</span><span class="token string">"Failed to get response"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
        <span class="token punctuation">}</span>
    <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
</li>
<li>
<p><strong>Create a Response Object (Optional):</strong><br>
If you want to map the JSON response to a Java object, create a POJO (Plain Old Java Object) that matches the structure of the JSON response.</p>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> <span class="token keyword">class</span> <span class="token class-name">MyResponseObject</span> <span class="token punctuation">{</span>
    <span class="token keyword">private</span> String someField<span class="token punctuation">;</span>
    <span class="token keyword">private</span> <span class="token keyword">int</span> anotherField<span class="token punctuation">;</span>

    <span class="token comment">// getters and setters</span>

    <span class="token keyword">public</span> String <span class="token function">getSomeField</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">return</span> someField<span class="token punctuation">;</span>
    <span class="token punctuation">}</span>

    <span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">setSomeField</span><span class="token punctuation">(</span>String someField<span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">this</span><span class="token punctuation">.</span>someField <span class="token operator">=</span> someField<span class="token punctuation">;</span>
    <span class="token punctuation">}</span>

    <span class="token keyword">public</span> <span class="token keyword">int</span> <span class="token function">getAnotherField</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">return</span> anotherField<span class="token punctuation">;</span>
    <span class="token punctuation">}</span>

    <span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">setAnotherField</span><span class="token punctuation">(</span><span class="token keyword">int</span> anotherField<span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">this</span><span class="token punctuation">.</span>anotherField <span class="token operator">=</span> anotherField<span class="token punctuation">;</span>
    <span class="token punctuation">}</span>

    <span class="token annotation punctuation">@Override</span>
    <span class="token keyword">public</span> String <span class="token function">toString</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">return</span> <span class="token string">"MyResponseObject{"</span> <span class="token operator">+</span>
                <span class="token string">"someField='"</span> <span class="token operator">+</span> someField <span class="token operator">+</span> <span class="token string">'\''</span> <span class="token operator">+</span>
                <span class="token string">", anotherField="</span> <span class="token operator">+</span> anotherField <span class="token operator">+</span>
                <span class="token string">'}'</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
</li>
<li>
<p><strong>Run Your Application:</strong><br>
Now you can run your Spring application, and it will make the API call and process the response accordingly.</p>
</li>
</ol>
<p>This example demonstrates how to use <code>RestTemplate</code> to make a GET request to a specified URL, retrieve the JSON response, and optionally map it to a Java object. Make sure you handle exceptions and errors as necessary for your specific use case.</p>

