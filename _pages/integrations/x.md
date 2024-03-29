---
permalink: /x
title: "Import trades from Twitter X into Trade Audit"
---
<h1 class="display-5 fw-bold mb-4 mt-5 text-center">Stock Recommendations made on<br>
<img src="/assets/integrations/x.svg" style="height:1em;" alt="Twitter X logo">
<span style="font-size:14px;vertical-align:middle;color:#657786">/</span>
<img src="/assets/integrations/twitter-logo.svg" style="height:1em;" alt="Twitter X logo">
</h1>

<div class="text-center lead">
The <span class="fst-italic">FinTwit</span> community on X is notorious for a lack of accountability.
</div>

<h2 class="display-6 fw-bold mb-4 mt-5 text-center">Platform Characteristics</h2>

<article class="facts">
    <section>
      <h3>❌ Deleting Bad Picks</h3>
      <p>
        By far the easiest way to keep a pristine track record is to delete bad tweets. There are few ways to recover deleted tweets
         so there is little risk of getting caught. Many X accounts automatically delete tweets over a few weeks old out of
        precaution and because they have no intention of being held to account.
      </p>
    </section>
    <section>
      <h3>❌ Playing Both Sides</h3>
      <p>
        A common strategy often combined with <span class="fw-bold">deleting bad picks</span>; is to play both sides of a trade. Pairs of posts are
        tweeted, separating the pro's and con's into separate tweets; capitalizing under the cover of X's character limit.
        Once a outcome is known, the wrong side of the trade's tweet is deleted, only leaving a fully correct prediction.
      </p>
    </section>
    <section>
      <h3>❌ Spam Bots</h3>
      <p>
        These play off of <a href="https://en.wikipedia.org/wiki/Survivorship_bias" target="_blank">survivorship bias</a>. Bot accounts
         with fake followers will make generic market calls in every sort of permutation.<br>
        It takes <span class="fw-bold">2<sup>N</sup></span> accounts to make <span class="fw-bold">N perfect market calls</span>,
        and X has 1.5 Million new accounts created every day.
      </p>
    </section>
</article>

{% include influencerlink.html %}

<h3 class="display-6 text-center mt-5">Critics of FinTwit Practices</h3>
<hr style="margin-top: 0;" class="mb-5">

<script type="module">
import {LitElement, html, svg, css} from '{{ site.litCoreJs }}';

export class TwitterTweet extends LitElement {
  //Populate using https://cdn.syndication.twimg.com/tweet-result?id=[id_str]&lang=en
  static properties = {
    user_name: { type: String },
    user_screen_name: { type: String },
    profile_image_url_https: { type: String },
    verified: { type: Boolean },
    id_str: { type: String },
    text: { type: String },
    display_url: { type: String },
    url: { type: String },
    created_at: { type: String },
  };
  
  static styles = css`
    * {
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen-Sans, Ubuntu, Cantarell, "Helvetica Neue", sans-serif;
    }

    #container {
      border-radius: 8px;
      overflow: hidden;
      border: 1px solid;
      margin-bottom: 2em;
      background-color: #fff;
    }
    
    @media only screen and (min-width: 1400px) {
      #container {
        width: 600px;
      }
    }

    #retweet {
      font-size: 0.75em;
      padding: 0 0 8px 44px;
      color: #657786;
    }

    #retweet a {
      color: #657786;
    }

    #retweet svg {
      width: 14px;
      height: 14px;
      vertical-align: middle;
      margin-bottom: 2px;
    }

    #content {
      padding: 24px 24px 20px 24px;
    }

    #profile-image {
      padding-right: 16px;
    }

    #profile-image img {
      border-radius: 50%;
      width: 48px;
      height: 48px;
    }

    #names {
      overflow: hidden;
    }

    #name {
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
      padding-right: 4px;
      font-size: 24px;
      font-weight: bold;
      line-height: 24px;
    }

    #media img, #media video {
      width: 100%;
      max-height: 400px;
      object-fit: cover;
      object-position: center;
    }

    #header {
      display: flex;
      justify-content: space-between;
    }

    #header-content {
      display: flex;
      overflow: hidden;
    }

    #text {
      margin-top: 16px;
      margin-bottom: 24px;
      font-size: 1.5em;
      line-height: 32px;
      letter-spacing: .01em;
      overflow-wrap: break-word;
    }

    #footer {
      display: flex;
      justify-content: space-between;
    }

    #footer #link {
      display: block;
      line-height: 30px;
    }

    #names {
      display: inline-flex;
      flex-direction: column;
    }

    #actions a {
      padding-right: 16px;
    }

    .icon {
      width: 20px;
      height: 20px;
    }

    a {
      color: var(--twitter-status-link-color);
      text-decoration: none;
      outline: 0;
    }

    a:visited {
      color: var(--twitter-status-link-color);
      text-decoration: none;
      outline: 0;
    }

    #actions svg {
      width: 30px;
      height: 30px;
    }

    #names svg {
      width: 18px;
      height: 18px;
    }

    #logo svg {
      width: 50px;
      height: 50px;
    }
  `;
  
  
  render() {
    
    function parseNewlines(body){
      const index = body.indexOf("\\n");
      if(index == -1) return body;
      else return html`${body.substring(0,index)}<br>${parseNewlines(body.substring(index + 2))}`;
    }
    
    function minUrl(url){
      var ret = url;
      if(ret.startsWith("https://")) ret = ret.substring(8);
      if(ret.startsWith("www.")) ret = ret.substring(4);
      if(ret.length > 22) ret = ret.substring(0,22) + "…";
      return ret;
    }
    
    const date = new Date(this.created_at);
    return html`
      <div id="container">
        <div id="content">
          <div id="header">
            <a id="header-content" target="_blank" rel="noopener" href="https://twitter.com/${ this.user_screen_name }">
              <span id="profile-image"><img src="${ this.profile_image_url_https }" alt="${ this.user_name }'s avatar'"></span>
              <span id="names">
                <span id="name">${ this.user_name }
                  ${ this.verified ? html`
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 64 72" class="verified"><path fill="none" d="M0 0h64v72H0z"></path><path fill="#1da1f2" d="M3 37.315c0 4.125 2.162 7.726 5.363 9.624-.056.467-.09.937-.09 1.42 0 6.103 4.72 11.045 10.546 11.045 1.295 0 2.542-.234 3.687-.686C24.22 62.4 27.827 64.93 32 64.93c4.174 0 7.782-2.53 9.49-6.213 1.148.45 2.39.685 3.69.685 5.826 0 10.546-4.94 10.546-11.045 0-.483-.037-.953-.093-1.42C58.83 45.04 61 41.44 61 37.314c0-4.37-2.42-8.15-5.933-9.946.427-1.203.658-2.5.658-3.865 0-6.104-4.72-11.045-10.545-11.045-1.302 0-2.543.232-3.69.688-1.707-3.685-5.315-6.216-9.49-6.216-4.173 0-7.778 2.53-9.492 6.216-1.146-.455-2.393-.688-3.688-.688-5.827 0-10.545 4.94-10.545 11.045 0 1.364.23 2.662.656 3.864C5.42 29.163 3 32.944 3 37.314z"></path><path fill="#FFF" d="M17.87 39.08l7.015 6.978c.585.582 1.35.873 2.116.873.77 0 1.542-.294 2.127-.883.344-.346 15.98-15.974 15.98-15.974 1.172-1.172 1.172-3.07 0-4.243-1.17-1.17-3.07-1.172-4.242 0l-13.87 13.863-4.892-4.868c-1.174-1.168-3.074-1.164-4.242.01-1.168 1.176-1.163 3.075.01 4.244z"></path></svg>
                   ` : null }
                </span>
                <span style="color:gray;font-size:0.9em;margin-top:-2px;">@${ this.user_screen_name }</span>
              </span>
            </a>
            <div id="logo">
              <a target="_blank" rel="noopener" href="https://twitter.com/${ this.user_screen_name }/status/${ this.id_str }">
                <!-- --><svg id="Logo_FIXED" data-name="Logo — FIXED" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 400 400"><defs><style>.cls-1{fill:none;}.cls-2{fill:#1da1f2;}</style></defs><title>Twitter_Logo_Blue</title><rect class="cls-1" width="400" height="400"></rect><path class="cls-2" d="M153.62,301.59c94.34,0,145.94-78.16,145.94-145.94,0-2.22,0-4.43-.15-6.63A104.36,104.36,0,0,0,325,122.47a102.38,102.38,0,0,1-29.46,8.07,51.47,51.47,0,0,0,22.55-28.37,102.79,102.79,0,0,1-32.57,12.45,51.34,51.34,0,0,0-87.41,46.78A145.62,145.62,0,0,1,92.4,107.81a51.33,51.33,0,0,0,15.88,68.47A50.91,50.91,0,0,1,85,169.86c0,.21,0,.43,0,.65a51.31,51.31,0,0,0,41.15,50.28,51.21,51.21,0,0,1-23.16.88,51.35,51.35,0,0,0,47.92,35.62,102.92,102.92,0,0,1-63.7,22A104.41,104.41,0,0,1,75,278.55a145.21,145.21,0,0,0,78.62,23"></path></svg>
              </a>
            </div>
          </div>
          <div id="text">
            ${ parseNewlines(this.text) }
            ${ this.display_url ? html`<a style="color:#1DA1F2;" href="${this.url}" title="${this.display_url}" target="_blank">${ minUrl(this.display_url) }</a><br>` : null }
          </div>
          <div id="footer">
            <div id="actions">
              <a target="_blank" rel="noopener" title="reply" href="https://twitter.com/intent/tweet?in_reply_to=${ this.id_str }">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path class="icon" fill="#657786" d="M14.046 2.242l-4.148-.01h-.002c-4.374 0-7.8 3.427-7.8 7.802 0 4.098 3.186 7.206 7.465 7.37v3.828c0 .108.045.286.12.403.143.225.385.347.633.347.138 0 .277-.038.402-.118.264-.168 6.473-4.14 8.088-5.506 1.902-1.61 3.04-3.97 3.043-6.312v-.017c-.006-4.368-3.43-7.788-7.8-7.79zm3.787 12.972c-1.134.96-4.862 3.405-6.772 4.643V16.67c0-.414-.334-.75-.75-.75h-.395c-3.66 0-6.318-2.476-6.318-5.886 0-3.534 2.768-6.302 6.3-6.302l4.147.01h.002c3.532 0 6.3 2.766 6.302 6.296-.003 1.91-.942 3.844-2.514 5.176z"></path></svg>
              </a>
              <a target="_blank" rel="noopener" title="retweet" href="https://twitter.com/intent/retweet?tweet_id=${ this.id_str }">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path fill="#657786" d="M23.77 15.67c-.292-.293-.767-.293-1.06 0l-2.22 2.22V7.65c0-2.068-1.683-3.75-3.75-3.75h-5.85c-.414 0-.75.336-.75.75s.336.75.75.75h5.85c1.24 0 2.25 1.01 2.25 2.25v10.24l-2.22-2.22c-.293-.293-.768-.293-1.06 0s-.294.768 0 1.06l3.5 3.5c.145.147.337.22.53.22s.383-.072.53-.22l3.5-3.5c.294-.292.294-.767 0-1.06zm-10.66 3.28H7.26c-1.24 0-2.25-1.01-2.25-2.25V6.46l2.22 2.22c.148.147.34.22.532.22s.384-.073.53-.22c.293-.293.293-.768 0-1.06l-3.5-3.5c-.293-.294-.768-.294-1.06 0l-3.5 3.5c-.294.292-.294.767 0 1.06s.767.293 1.06 0l2.22-2.22V16.7c0 2.068 1.683 3.75 3.75 3.75h5.85c.414 0 .75-.336.75-.75s-.337-.75-.75-.75z"></path></svg>
              </a>
              <a target="_blank" rel="noopener" title="like" href="https://twitter.com/intent/like?tweet_id=${ this.id_str }">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path fill="#657786" d="M12 21.638h-.014C9.403 21.59 1.95 14.856 1.95 8.478c0-3.064 2.525-5.754 5.403-5.754 2.29 0 3.83 1.58 4.646 2.73.813-1.148 2.353-2.73 4.644-2.73 2.88 0 5.404 2.69 5.404 5.755 0 6.375-7.454 13.11-10.037 13.156H12zM7.354 4.225c-2.08 0-3.903 1.988-3.903 4.255 0 5.74 7.035 11.596 8.55 11.658 1.52-.062 8.55-5.917 8.55-11.658 0-2.267-1.822-4.255-3.902-4.255-2.528 0-3.94 2.936-3.952 2.965-.23.562-1.156.562-1.387 0-.015-.03-1.426-2.965-3.955-2.965z"></path></svg>
              </a>
            </div>
            <div id="link">
              <a target="_blank" rel="noopener" href="https://twitter.com/${ this.user_screen_name }/status/${ this.id_str }">
                ${ date.toLocaleTimeString(undefined, { timeStyle: 'short' }) } · ${ date.toLocaleDateString(undefined, { dateStyle: 'medium' }) }
              </a>
            </div>
          </div>
        </div>
      </div>
    `;
  }
}
customElements.define('twitter-tweet', TwitterTweet);
</script>

<style>
  @media only screen and (min-width: 1400px) {
    .masonry {
      columns: 600px;
      column-gap: 1rem;
    }
  }
</style>

<div class="masonry">
  <!-- https://cdn.syndication.twimg.com/tweet-result?id=463440424141459456 -->
  <twitter-tweet 
    user_screen_name="RudyHavenstein"
    user_name="Rudy Havenstein"
    id_str="1586061354271526912"
    profile_image_url_https="https://tradeaudit.app/assets/x_profile_images/et3kkNBx_400x400.jpg"
    text="Fintwit really is a delusional place."
    created_at="2022-10-28T18:24:29.000Z"
  ></twitter-tweet>
  
  <twitter-tweet 
    user_screen_name="SoccerMomTrades"
    user_name="Canadian Soccer Mom"
    id_str="1504853649746538504"
    profile_image_url_https="https://tradeaudit.app/assets/x_profile_images/GHDrzL6G_400x400.jpg"
    text="I can just picture it, couple of months from now when market is fine the mega macro newsletters coming out with some piece like “So what happened?” being really retrospective and maybe quote a bunch of lines from drunkmiller and soros and boom they move on\n\nMeanwhile subs rekt"
    created_at="2022-03-18T16:14:03.000Z"
  ></twitter-tweet>
  
  <twitter-tweet 
    user_screen_name="respeculator"
    user_name="Respeculator"
    id_str="1570699224383385601"
    profile_image_url_https="https://tradeaudit.app/assets/x_profile_images/W7lnCdPH_400x400.jpg"
    text="Twitter is full of big ‘macro’ accounts that either make ambiguous calls that sound smart &amp; are never wrong “misinterpreted what I was saying” or make big calls but then shift the goal posts re timing. Never wrong though. Block these macro furu’s and focus on what ur skillset is"
    created_at="2022-09-16T09:00:51.000Z"
  ></twitter-tweet>
  
  <div></div>
  
  <twitter-tweet 
    user_screen_name="Guruleaks1"
    user_name="GuruLeaks"
    id_str="1522231095416262656"
    profile_image_url_https="https://tradeaudit.app/assets/x_profile_images/cky2QGzY_400x400.jpg"
    text="What are those LOUD SOUNDS you ask?\n\nThat's the sound of HUNDREDS OF GURUS deleting their bullish tweets from yesterday's close and replacing them with bearish tweets now"
    created_at="2022-05-05T15:05:49.000Z"
  ></twitter-tweet>

  <twitter-tweet 
    user_screen_name="WifeyAlpha"
    user_name="Wifey"
    id_str="1541754905635049474"
    profile_image_url_https="https://tradeaudit.app/assets/x_profile_images/VQNgdgtC_400x400.jpg"
    text="ppl on fintwit trade the wrong securities, they take the wrong side/direction and more important they get the trading frequency diversification wrong\n\nand after monumentally fucking up they lie and say they did not trade or they delete tweets ... just incredible"
    created_at="2022-06-28T12:06:28.000Z"
  ></twitter-tweet>
  
  <twitter-tweet 
    user_screen_name="alexkagin"
    user_name="Alex Kagin"
    id_str="1466026950791766020"
    profile_image_url_https="https://tradeaudit.app/assets/x_profile_images/oUiwc1Uc_400x400.jpg"
    text="Do you think most fintwit newsletter writers hold stocks they publish? I’m thinking maybe 20%"
    created_at="2021-12-01T12:50:37.000Z"
  ></twitter-tweet>
  
</div>

<h3 class="display-6 text-center mt-5">Fraud on FinTwit</h3>
<hr style="margin-top: 0;" class="mb-5">

<div class="masonry">
  <twitter-tweet 
    user_screen_name="SECGov"
    user_name="U.S. Securities and Exchange Commission"
    id_str="1603036089899327488"
    profile_image_url_https="https://tradeaudit.app/assets/x_profile_images/9nXj4OYd_400x400.jpg"
    text="Today we announced charges against eight social media influencers in a $100 million securities scheme in which they used Twitter and Discord to manipulate exchange-traded stocks.\n\n"
    display_url="https://www.sec.gov/news/press-release/2022-221"
    url="https://t.co/zzrchqFlqg"
    created_at="2022-12-14T14:36:01.000Z"
  ></twitter-tweet>

  <twitter-tweet 
    user_screen_name="notoriousalerts"
    user_name="Gary (Mystic Mac) 🍀"
    id_str="1550116715337371651"
    profile_image_url_https="https://tradeaudit.app/assets/x_profile_images/7mpcym1k_400x400.jpg"
    text="No. You closed my account. All good will transfer 10 million elsewhere today. It was a pleasure. All the best 💚✌🏼"
    created_at="2022-07-21T13:51:38.000Z"
  ></twitter-tweet>
  
  <!--
  <twitter-tweet 
    user_screen_name="MrZackMorris"
    user_name="Zack Morris"
    id_str="1602915568612478977"
    profile_image_url_https="https://tradeaudit.app/assets/x_profile_images/L-peQt77_400x400.jpg"
    text="I love my homies on here. The rest of you can keep swinging on my nuts."
    created_at="2022-12-14T06:37:06.000Z"
  ></twitter-tweet>-->
  
  <twitter-tweet 
    user_screen_name="LadeBackk"
    user_name="LADE BACKK"
    id_str="1600162340908912643"
    profile_image_url_https="https://tradeaudit.app/assets/x_profile_images/gwZ78sFp_400x400.jpg"
    text="These low floats around a dime are hot 🔥"
    created_at="2022-12-06T16:16:46.000Z"
  ></twitter-tweet>
  
  <div></div>
  
  <twitter-tweet 
    user_screen_name="MrZackMorris"
    user_name="Zack Morris"
    id_str="1595994913711489025"
    profile_image_url_https="https://tradeaudit.app/assets/x_profile_images/L-peQt77_400x400.jpg"
    text="Tomorrow low floats, cheap pennies and the market will pump. Make your money quick and enjoy your weekend. ALWAYS MANAGE YOUR RISK."
    created_at="2022-11-25T04:16:54.000Z"
  ></twitter-tweet>
  
  <twitter-tweet 
    user_screen_name="ohheytommy"
    user_name="Tommy Coops"
    id_str="1598156982862909440"
    profile_image_url_https="https://tradeaudit.app/assets/x_profile_images/9b7GBTng_400x400.jpg"
    text="Nailed $SPY and $AAPL both directions today. I truly love nothing more than helping people . I hope in some way shape or form I’ve been a positive for you🙏🏻"
    created_at="2022-12-01T03:28:11.000Z"
  ></twitter-tweet>
  
  <twitter-tweet 
    user_screen_name="DipDeity"
    user_name="Dan, Deity of Dips"
    id_str="1580655346368274432"
    profile_image_url_https="https://tradeaudit.app/assets/x_profile_images/zDIobrdd_400x400.jpg"
    text="Fintwit’s back\n\n@MrZackMorris @PJ_Matlock"
    display_url="📷"
    url="https://t.co/BFixnShXUy"
    created_at="2022-10-13T20:22:56.000Z"
  ></twitter-tweet>
  
</div>

<hr style="margin-top: 0;" class="mb-5">

{% include influencerlink.html %}
