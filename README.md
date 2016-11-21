# BubbleButton
button with bubble for counting(like in Cart)

import UIKit

public class BubbleButton: UIButton {
    
    private var bubble: UILabel
    public var bubbleText: String? {
        didSet {
            setupBubbleView(withBubbleString: bubbleText)
        }
    }
    
    public var bubbleTextColor: UIColor = .white {
        didSet {
            bubble.textColor = bubbleTextColor
        }
    }
    
    public var bubbleBackgroundColor = UIColor(red: 208/255.0, green: 2/255.0, blue: 27/255.0, alpha: 1) {
        didSet {
            bubble.backgroundColor = bubbleBackgroundColor
        }
    }
    
    public var bubbleEdgeInsets: UIEdgeInsets? {
        didSet {
            setupBubbleView(withBubbleString: bubbleText)
        }
    }
    
    override public init(frame: CGRect) {
        bubble = UILabel()
        super.init(frame: frame)
        setupBubbleView(withBubbleString: "")
    }
    
    required public init?(coder aDecoder: NSCoder) {
        bubble = UILabel()
        super.init(coder: aDecoder)
        setupBubbleView(withBubbleString: "")
    }
    
    public func initWithFrame(frame: CGRect, withBubbleString bubbleString: String, withBubbleInsets bubbleInsets: UIEdgeInsets) -> AnyObject {
        
        bubble = UILabel()
        bubbleEdgeInsets = bubbleInsets
        setupBubbleView(withBubbleString: bubbleText)
        return self
    }
    
    private func setupBubbleView(withBubbleString bubbleString: String?) {
        bubble.clipsToBounds = true
        bubble.text = bubbleString
        bubble.font = UIFont.systemFont(ofSize: 12)
        bubble.textAlignment = .center
        bubble.sizeToFit()
        let bubbleSize = bubble.frame.size
        
        let height = max(20, Double(bubbleSize.height) + 5.0)
        let width = max(height, Double(bubbleSize.width) + 10.0)
        
        var vertical: Double?, horizontal: Double?
        if let bubbleInset = self.bubbleEdgeInsets {
            vertical = Double(bubbleInset.top) - Double(bubbleInset.bottom)
            horizontal = Double(bubbleInset.left) - Double(bubbleInset.right)
            
            let x = (Double(bounds.size.width) - 10 + horizontal!)
            let y = -(Double(bubbleSize.height) / 2) - 10 + vertical!
            bubble.frame = CGRect(x: x, y: y, width: width, height: height)
        } else {
            let x = self.frame.width - CGFloat((width / 2.0))
            let y = CGFloat(-(height / 2.0))
            bubble.frame = CGRect(x: x, y: y, width: CGFloat(width), height: CGFloat(height))
        }
        
        setupBubbleStyle()
        addSubview(bubble)
        
        bubble.isHidden = bubbleString != nil ? false : true
    }
    
    private func setupBubbleStyle() {
        bubble.textAlignment = .center
        bubble.backgroundColor = bubbleBackgroundColor
        bubble.textColor = bubbleTextColor
        bubble.layer.cornerRadius = bubble.bounds.size.height / 2
    }
}
