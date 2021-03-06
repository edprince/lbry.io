---
title: How do I see the list of API functions I can call, and how do I call them?
category: developer
---

Ensure that `lbrycrd` is running with the `-server` flag, which enables the JSON-RPC API. Then use one of the following methods to make API calls.

Many (though not all) of the calls are the same as those for bitcoin core, which are
documented [here](https://en.bitcoin.it/wiki/Original_Bitcoin_client/API_calls_list). To see the full list of API calls, use the `help` API call.

## lbrycrd-cli

    lbrycrd-cli help

## curl

    curl --user USER:PASSWORD --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "help", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:9245/

- `USER` and `PASSWORD` can be found in your lbrycrd.conf file.
- The `method` field can be any of the supported methods like `getbalance` or `getnewaddress`.
- `9245` is the default port used, but if you chose a custom port for the server, you'll need to use that instead.
- If the command accepts parameters, they can be passed inside the `params` array.

See Also: [important directories](https://lbry.io/faq/lbry-directories).

## Python

Here is an example script to get the documentation for the various API calls.
To use any of the functions displayed, just provide any specified arguments in a dictionary.

    import sys
    from jsonrpc.proxy import JSONRPCProxy

    try:
      from lbrynet.conf import API_CONNECTION_STRING
    except:
      print "You don't have lbrynet installed!"
      sys.exit(0)

    api = JSONRPCProxy.from_url(API_CONNECTION_STRING)
    if not api.is_running():
      print api.daemon_status()
    else:
      for func in api.help():
        print "%s:\n%s" % (func, api.help({'function': func}))
