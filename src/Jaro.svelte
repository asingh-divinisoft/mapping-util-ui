<script context="module">
    const JaroWrinker = (s1, s2) => {
        let m = 0;

        // Exit early if either are empty.
        if ( s1.length === 0 || s2.length === 0 ) {
            return 0;
        }

        // Exit early if they're an exact match.
        if ( s1 === s2 ) {
            return 1;
        }

        let range     = (Math.floor(Math.max(s1.length, s2.length) / 2)) - 1,
            s1Matches = new Array(s1.length),
            s2Matches = new Array(s2.length);
        
        let i=0;
        let j=0;
        for ( i = 0; i < s1.length; i++ ) {
            let low  = (i >= range) ? i - range : 0,
                high = (i + range <= s2.length) ? (i + range) : (s2.length - 1);

            for ( j = low; j <= high; j++ ) {
            if ( s1Matches[i] !== true && s2Matches[j] !== true && s1[i] === s2[j] ) {
                ++m;
                s1Matches[i] = s2Matches[j] = true;
                break;
            }
            }
        }

        // Exit early if no matches were found.
        if ( m === 0 ) {
            return 0;
        }

        // Count the transpositions.
        let k = 0;
        let n_trans = 0;

        for ( i = 0; i < s1.length; i++ ) {
            if ( s1Matches[i] === true ) {
            for ( j = k; j < s2.length; j++ ) {
                if ( s2Matches[j] === true ) {
                k = j + 1;
                break;
                }
            }

            if ( s1[i] !== s2[j] ) {
                ++n_trans;
            }
            }
        }

        let weight = (m / s1.length + m / s2.length + (m - (n_trans / 2)) / m) / 3,
            l = 0,
            p = 0.1;

        if ( weight > 0.7 ) {
            while ( s1[l] === s2[l] && l < 4 ) {
            ++l;
            }

            weight = weight + l * p * (1 - weight);
        }

        return weight;
    }

    export const naiveSort = (a,b) => {
        let fa = a.text.toLowerCase();
        let fb = b.text.toLowerCase();
        if (fa < fb) {
            return -1;
        }
        if (fa > fb) {
            return 1;
        }
        return 0;
    }

    export const bigSort = (o1, o2, common) => {
        // console.log(o1, o2, common);
        let v1 = JaroWrinker(o1, common);
        let v2 = JaroWrinker(o2, common);
        // console.log('sort', v1, v2);
        return v2-v1;
    };
</script>