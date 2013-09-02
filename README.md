Message Module
==============

This is the Message module for Pi.

User private messages and the system notification are provided.

Note
====
- You should deny guest to access this module by editing permissions in backend.

For consumer
============
1. Client register
  - <host>/oauth/client/register
      - bbb
  - abc

2. Get authorization code
  - Url: <host>/oauth/authorize/index
  - method: GET
  - parameters
    || *client_id* || Client ID assigned after register the client ||
    || *response_type* || Response type, the value is "code" fixed. ||
    || *redirect_uri* || Callback uri after authorization ||
    || *state* || State of client, it will be sent back after authorization ||
    || *scope(optional)* || Uncompleted ||
