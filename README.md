<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  Amani API documentation
  <link rel="stylesheet" href="https://stackedit.io/style.css" />
</head>

<body class="stackedit">
  <div class="stackedit__html"><h1 id="amani-cv">Amani API</h1>
<p>Amani API’s endpoints can be categorized as 3 groups:</p>
<ul>
<li>Authorization &amp; Authentication</li>
<li>Uploading document</li>
<li>Querying customer data and results</li>
</ul>
<p>You should follow authorization mechanics in all of your requests except the login will provide a token for you.</p>
<h2 id="auth">Auth</h2>
<p>Amani API uses JWT Auth in multiple layers. You basically get a token and use it till its expiration.</p>
<p>You should obtain your token by using the request below:</p>
<pre class=" language-http"><code class="prism  language-http"><span class="token header-name keyword">HOST:</span> https://tr.amani.ai/api/v1/user/login/
<span class="token header-name keyword">Method:</span> POST
<span class="token header-name keyword">Content-Type:</span> application/json<span class="token application/json">

<span class="token punctuation">{</span>
    <span class="token string">"email"</span><span class="token punctuation">:</span> <span class="token string">"registeredemail@email.com"</span><span class="token punctuation">,</span>
    <span class="token string">"password"</span><span class="token punctuation">:</span> <span class="token string">"VERYSTRONGPASSWORD"</span>
<span class="token punctuation">}</span>
</span></code></pre>
<p>This will give you an answer that contains your token:</p>
<pre class=" language-json"><code class="prism  language-json"><span class="token punctuation">{</span>
    <span class="token string">"token"</span><span class="token punctuation">:</span> <span class="token string">"YOUR.JWT.TOKEN"</span><span class="token punctuation">,</span>
    <span class="token string">"access_hash"</span><span class="token punctuation">:</span> <span class="token string">"b0b7b4b6-6de5-4b9b-9b1d-78be78c7975b"</span><span class="token punctuation">,</span>
    <span class="token string">"groups"</span><span class="token punctuation">:</span> <span class="token punctuation">[</span><span class="token string">"company admin"</span><span class="token punctuation">]</span><span class="token punctuation">,</span>
    <span class="token string">"name"</span><span class="token punctuation">:</span> <span class="token string">"FirstName LastName"</span><span class="token punctuation">,</span>
    <span class="token string">"email"</span><span class="token punctuation">:</span> <span class="token string">"registeredemail@email.com"</span><span class="token punctuation">,</span>
    <span class="token string">"company_id"</span><span class="token punctuation">:</span> <span class="token number">1</span><span class="token punctuation">,</span>
    <span class="token string">"company_name"</span><span class="token punctuation">:</span> <span class="token string">"Name of the Company"</span><span class="token punctuation">,</span>
    <span class="token string">"available_documents"</span><span class="token punctuation">:</span> <span class="token punctuation">[</span><span class="token string">"ID"</span><span class="token punctuation">,</span> <span class="token string">"PA"</span><span class="token punctuation">,</span> <span class="token string">"VA"</span><span class="token punctuation">,</span> <span class="token string">"UB"</span><span class="token punctuation">,</span> <span class="token string">"IB"</span><span class="token punctuation">,</span> <span class="token string">"SE"</span><span class="token punctuation">,</span> <span class="token string">"SG"</span><span class="token punctuation">]</span>
<span class="token punctuation">}</span>
</code></pre>
<p>For further usages, you only need the <code>token</code> parameter (Will be noted as <code>TOKEN</code>).  <code>access_hash</code> parameter is only for backward compatibility, deprecated and will be removed later.</p>
<h2 id="customer-creation">Customer Creation</h2>
<p>After login, you need to create a customer in order to upload documents. To do this, follow the request below:</p>
<pre class=" language-http"><code class="prism  language-http"><span class="token header-name keyword">HOST:</span> https://tr.amani.ai/api/v1/customer/
<span class="token header-name keyword">Method:</span> POST
<span class="token header-name keyword">Content-Type:</span> application/json
<span class="token header-name keyword">Authorization:</span> Token YOUR.JWT.TOKEN<span class="token application/json">

<span class="token punctuation">{</span>
	<span class="token string">"id_card_number"</span><span class="token punctuation">:</span> <span class="token string">"12345678912"</span><span class="token punctuation">,</span>
	<span class="token string">"name"</span><span class="token punctuation">:</span> <span class="token string">"Example Name"</span><span class="token punctuation">,</span>
	<span class="token string">"email"</span><span class="token punctuation">:</span> <span class="token string">"example@email.com"</span><span class="token punctuation">,</span>
	<span class="token string">"phone"</span><span class="token punctuation">:</span> <span class="token string">"+90 123 123 12 12"</span>
<span class="token punctuation">}</span>
</span></code></pre>
<p>You should get a response like below:</p>
<pre class=" language-json"><code class="prism  language-json"><span class="token punctuation">{</span>
    <span class="token string">"id"</span><span class="token punctuation">:</span> <span class="token number">465</span><span class="token punctuation">,</span>
    <span class="token string">"name"</span><span class="token punctuation">:</span> <span class="token string">"Example Name"</span><span class="token punctuation">,</span>
    <span class="token string">"phone"</span><span class="token punctuation">:</span> <span class="token string">"+90 123 123 12 12"</span><span class="token punctuation">,</span>
    <span class="token string">"email"</span><span class="token punctuation">:</span> <span class="token string">"example@email.com"</span><span class="token punctuation">,</span>
    <span class="token string">"token"</span><span class="token punctuation">:</span> <span class="token string">"CUSTOMER_TOKEN (DEPRECATED)"</span><span class="token punctuation">,</span>
    <span class="token string">"id_card_number"</span><span class="token punctuation">:</span> <span class="token string">"12345678912"</span>
<span class="token punctuation">}</span>
</code></pre>
<p>After this point, you’ll be able to upload documents for that customer.</p>
<h2 id="uploading-a-document">Uploading a Document</h2>
<p>Because you’ll send a file via this request, content type changes to <code>multipart/form-data</code>.  Please follow the request below:</p>
<pre class=" language-http"><code class="prism  language-http"><span class="token header-name keyword">HOST:</span> https://tr.amani.ai/api/v1/recognition/web/upload
<span class="token header-name keyword">Method:</span> POST
<span class="token header-name keyword">Content-Type:</span> multipart/form-data; boundary=----someboundary
<span class="token header-name keyword">Authorization:</span> Token YOUR.JWT.TOKEN

<span class="token header-name keyword">type:</span> PA
<span class="token header-name keyword">customer_id:</span> 465
files[]: data:image/jpeg;base64,IMAGEDATA0
files[]: data:image/jpeg;base64,IMAGEDATA1
</code></pre>
<p><code>files[]</code> parameter consists of your document’s  pages. All pages of the document should be here and in order.</p>
<p>After uploading, you’ll get the response</p>
<pre class=" language-json"><code class="prism  language-json"><span class="token punctuation">{</span>
	<span class="token string">"status"</span><span class="token punctuation">:</span> <span class="token string">"OK"</span><span class="token punctuation">,</span>
	<span class="token string">"accepted_documents"</span><span class="token punctuation">:</span> <span class="token punctuation">[</span><span class="token string">"ID"</span><span class="token punctuation">]</span>
<span class="token punctuation">}</span>
</code></pre>
<p>This response informs you about the uploaded document. If the document is not classified you’ll see <code>"XX"</code> instead of <code>"ID"</code><br>
If you see a different response, there is an issue. Please contact us so we can investigate.</p>
</div>
</body>

</html>
