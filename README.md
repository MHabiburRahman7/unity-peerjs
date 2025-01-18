# unity-peerjs
plugin, what implemented peerjs in unity for webgl

# Requirement
1. Unity Webgl Build (It won't work on editor)
2. Tested on 2021, 2022, 6

# How to use

## Installation
1. Open Unity Engine Package Manager -> Add (+) -> Install Package from Git URL
2. Fill with this Repository's git url `https://github.com/MHabiburRahman7/unity-peerjs.git`
3. Run the peerJS server [Reference](https://github.com/peers/peerjs-server)

## Integration
```
using UnityEngine;
using static Ramon171.UnityPeerJS.Runtime.UnityPeerJS;

public class PeerJSController : MonoBehaviour
{
    Peer.IConnection createdConnection = null;
    Peer peer = null;
    const string peerJSKey = "peerjs";
    string myPeerId = "peer0";
    const string peerJSServerHostName = "127.0.0.1";
    const int peerJSServerPort = 9000;

    void Start()
    {
        InitPeer();
    }

    public void InitPeer()
    {
        Debug.Log($"Creating new connection with id {myPeerId}");
        peer = new Peer(peerJSKey, myPeerId, peerJSServerHostName, peerJSServerPort);
        
        peer.OnConnection += conn => 
        {
            createdConnection = conn;
            createdConnection.OnData += message =>
            {
                Debug.Log($"Received Message {message}");
            };
        };

        peer.OnOpen += () =>
        {
            Debug.Log($"It is now opened");
        };

        peer.OnClose += () => 
        {
            Debug.Log($"It is now closed");
        };

        peer.OnError += err => 
        {
            Debug.LogError($"It is now error {err}");
        };

        peer.OnDisconnected += () => 
        {
            Debug.Log($"It is now Disconnected");
        };
    }

    public void JoinOtherNetwork()
    {
        if(peer != null)
        {
            var remoteId = "peer1";
            Debug.Log($"Attempting to connect to {remoteId}");
            peer.Connect(remoteId);
        }
    }

    public void SendMessage()
    {
        if (peer != null)
        {
            var message = "Hello world";
            Debug.Log($"Attempting to send {message}");
            createdConnection.Send(message);
        }
    }

    public void CloseConn()
    {
        if (peer != null)
        {
            Debug.Log($"Attempting to close");
            createdConnection.Close();
        }
    }

    void Update()
    {
        if(peer != null)
        {
            peer.Pump();
        }
    }
}
```
