---
id: version-0.46-appstate
original_id: appstate
title: appstate
---
<a id="content"></a><h1><a class="anchor" name="appstate"></a>AppState <a class="hash-link" href="docs/appstate.html#appstate">#</a></h1><div><div><p><code>AppState</code> can tell you if the app is in the foreground or background,
and notify you when the state changes.</p><p>AppState is frequently used to determine the intent and proper behavior when
handling push notifications.</p><h3><a class="anchor" name="app-states"></a>App States <a class="hash-link" href="docs/appstate.html#app-states">#</a></h3><ul><li><code>active</code> - The app is running in the foreground</li><li><code>background</code> - The app is running in the background. The user is either
 in another app or on the home screen</li><li><code>inactive</code> - This is a state that occurs when transitioning between
 foreground &amp; background, and during periods of inactivity such as
 entering the Multitasking view or in the event of an incoming call</li></ul><p>For more information, see
<a href="https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/TheAppLifeCycle/TheAppLifeCycle.html" target="_blank">Apple's documentation</a></p><h3><a class="anchor" name="basic-usage"></a>Basic Usage <a class="hash-link" href="docs/appstate.html#basic-usage">#</a></h3><p>To see the current state, you can check <code>AppState.currentState</code>, which
will be kept up-to-date. However, <code>currentState</code> will be null at launch
while <code>AppState</code> retrieves it over the bridge.</p><div class="prism language-javascript"><span class="token keyword">import</span> React<span class="token punctuation">,</span> <span class="token punctuation">{</span>Component<span class="token punctuation">}</span> <span class="token keyword">from</span> <span class="token string">'react'</span>
<span class="token keyword">import</span> <span class="token punctuation">{</span>AppState<span class="token punctuation">,</span> Text<span class="token punctuation">}</span> <span class="token keyword">from</span> <span class="token string">'react-native'</span>

<span class="token keyword">class</span> <span class="token class-name">AppStateExample</span> <span class="token keyword">extends</span> <span class="token class-name">Component</span> <span class="token punctuation">{</span>

  state <span class="token operator">=</span> <span class="token punctuation">{</span>
    appState<span class="token punctuation">:</span> AppState<span class="token punctuation">.</span>currentState
  <span class="token punctuation">}</span>

  <span class="token function">componentDidMount</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    AppState<span class="token punctuation">.</span><span class="token function">addEventListener</span><span class="token punctuation">(</span><span class="token string">'change'</span><span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">.</span>_handleAppStateChange<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>

  <span class="token function">componentWillUnmount</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    AppState<span class="token punctuation">.</span><span class="token function">removeEventListener</span><span class="token punctuation">(</span><span class="token string">'change'</span><span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">.</span>_handleAppStateChange<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>

  _handleAppStateChange <span class="token operator">=</span> <span class="token punctuation">(</span>nextAppState<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
    <span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token keyword">this</span><span class="token punctuation">.</span>state<span class="token punctuation">.</span>appState<span class="token punctuation">.</span><span class="token function">match</span><span class="token punctuation">(</span><span class="token regex">/inactive|background/</span><span class="token punctuation">)</span> <span class="token operator">&amp;&amp;</span> nextAppState <span class="token operator">===</span> <span class="token string">'active'</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
      console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'App has come to the foreground!'</span><span class="token punctuation">)</span>
    <span class="token punctuation">}</span>
    <span class="token keyword">this</span><span class="token punctuation">.</span><span class="token function">setState</span><span class="token punctuation">(</span><span class="token punctuation">{</span>appState<span class="token punctuation">:</span> nextAppState<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>

  <span class="token function">render</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token keyword">return</span> <span class="token punctuation">(</span>
      <span class="token operator">&lt;</span>Text<span class="token operator">&gt;</span>Current state is<span class="token punctuation">:</span> <span class="token punctuation">{</span><span class="token keyword">this</span><span class="token punctuation">.</span>state<span class="token punctuation">.</span>appState<span class="token punctuation">}</span><span class="token operator">&lt;</span><span class="token operator">/</span>Text<span class="token operator">&gt;</span>
    <span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>

<span class="token punctuation">}</span></div><p>This example will only ever appear to say "Current state is: active" because
the app is only visible to the user when in the <code>active</code> state, and the null
state will happen only momentarily.</p></div><span><h3><a class="anchor" name="methods"></a>Methods <a class="hash-link" href="docs/appstate.html#methods">#</a></h3><div class="props"><div class="prop"><h4 class="methodTitle"><a class="anchor" name=""></a>=<span class="methodType">(;, ()</span> <a class="hash-link" href="docs/appstate.html#">#</a></h4></div><div class="prop"><h4 class="methodTitle"><a class="anchor" name="addeventlistener"></a>addEventListener<span class="methodType">(type, handler)</span> <a class="hash-link" href="docs/appstate.html#addeventlistener">#</a></h4><div><p>Add a handler to AppState changes by listening to the <code>change</code> event type
and providing the handler</p><p>TODO: now that AppState is a subclass of NativeEventEmitter, we could deprecate
<code>addEventListener</code> and <code>removeEventListener</code> and just use <code>addListener</code> and
<code>listener.remove()</code> directly. That will be a breaking change though, as both
the method and event names are different (addListener events are currently
required to be globally unique).</p></div></div><div class="prop"><h4 class="methodTitle"><a class="anchor" name="removeeventlistener"></a>removeEventListener<span class="methodType">(type, handler)</span> <a class="hash-link" href="docs/appstate.html#removeeventlistener">#</a></h4><div><p>Remove a handler by passing the <code>change</code> event type and the handler</p></div></div></div></span></div><p class="edit-page-block"><a target="_blank" href="https://github.com/facebook/react-native/blob/master/Libraries/AppState/AppState.js">Improve this page</a> by sending a pull request!</p><div class="docs-prevnext"><a class="docs-prev" href="docs/appregistry.html#content">← Prev</a><a class="docs-next" href="docs/asyncstorage.html#content">Next →</a></div>