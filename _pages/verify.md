---
title: "Verify"
permalink: "/verify.html"
---

<form method="">    
<p class="mb-4">Please paste the text & Signature to check, Are they verified?!</p>
<div class="form-group row">
<textarea rows="8" class="form-control mb-3" id="message" name="message" placeholder="Text*" required></textarea> 
</div>
<div class="form-group row">
<input class="form-control" pattern="[A-Za-z0-9+/=]{344}" type="text" id="sign" name="sign" placeholder="Signature*" pattern="" required>
</div>
<input class="btn btn-success" id="verify" type="button" value="Send">
</form>

<script>
    $(document).on('click','#verify',function(e) {
        console.log("hi");
        const originalData = $("#message").val();
        const signature = $("#sign").val();;

        // Public key received from the server
        const publicKey = 'MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAl/f9h0PC2cmr73vhXrtPnG+xQ2JKJIkkmx2WZGiUEnrU+/k4CgoPeif59tOiL9RxB2zeKLh1bOogvllVF1hxASbMnyOAhZukMmJaueKs0QX1lh/P0BnFXYLBwcKsOrmfppy67h+WBx7lI5IEOVqtL1+YZ0glLzBlJ11HOdb/2NYYuhGppJDhcsXlcXLNuiB4ha35vGWv1K7KHGFryneCs1ng1QBQwSf0ivj2UugOdab2Hr8kUo1wjmRSMWYrJ3hPmLpz2RLss94eKbHK3GZt4P4AhitBiPtM7Pvtq6GgYSh5icmlvn7JNtYWDGtLm2rFgLXDZjBdQ6I61su+8hshUwIDAQAB'; // Replace with the actual public key

        // Convert the signature and public key from Base64 to Uint8Array
        const signatureBytes = Uint8Array.from(atob(signature), c => c.charCodeAt(0));
        const publicKeyBytes = Uint8Array.from(atob(publicKey), c => c.charCodeAt(0));

        // Convert the original data to a Uint8Array
        const dataBytes = new TextEncoder().encode(originalData);

        // Verify the signature
        (async () => {
        try {
            const key = await crypto.subtle.importKey(
            'spki',
            publicKeyBytes,
            {
                name: 'RSASSA-PKCS1-v1_5',
                hash: 'SHA-256',
            },
            false,
            ['verify']
            );

            const isVerified = await crypto.subtle.verify(
            'RSASSA-PKCS1-v1_5',
            key,
            signatureBytes,
            dataBytes
            );

            if(isVerified){
                alert("It is Verified");
            } else{
                alert("It is not Verified");
            }
        } catch (error) {
            console.error('Error:', error);
        }
        })();

    });
</script>